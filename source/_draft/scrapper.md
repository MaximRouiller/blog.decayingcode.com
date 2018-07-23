---
title : "Cloud Scale API scraping with serverless orchestrators"
date:
tags: [azure,serverless]
---

One of the things I'm working on requires me to retrieve information from multiple sources like the NuGet API and GitHub API. Let's take a concrete example of one of the things I did recently. If you follow me [on Twitter](https://twitter.com/MaximRouiller), you probably already know about it.

There's tons of Samples on the [Azure-Samples](https://github.com/Azure-Samples/) organization on GitHub, and I want to be able to check them out to see which ones are "too old". For example, what about a sample that was last modified 1 month ago? What about 3 months or 6 months? What if the sample is over a year old? Is it relevant?

# Our scenario

Ever ended up on a sample that should be covering the problem you're having but it just doesn't work? Then you check the last commit date only to realize that it's been 2 years since the last commit. Then, you check the packages and realize that they are 3 major versions behind. That sample is almost no good to you.

Well, the sample repositories on the Azure-Samples organization have the exact issues, and it's one of the many problems that I'm trying to solve. However, we first need to be able to retrieve all that information.

# File -> New Project -> Console Application

Our first instinct as programmers is to try to do it once through a console application. It's the minimum amount of complexity. So, I went ahead and started by consulting the [GitHub API documentation](https://developer.github.com/v3/), created a GitHub token and started hunting for the information I was looking for.

# First problem: Running locally

So, everything was running fine but the problem for me at that point was it was just a single console application running locally. I could have easily taken it to containers and be done with it, but I saw another way to solve this. I managed to migrate everything into an Azure Function. The problem now was that it was still just a console application running inside an Azure Functions. Lame.

For starters, once you know all the repositories it's a distributed problem. How many repositories can I hit at once without having any state to correlate? The answer is all of them. But first, I'd need to refactor.

# Introducing Durable Functions

So we all know what serverless is. If you need a refresher, you can review the [Azure Functions Overview](https://docs.microsoft.com/azure/azure-functions/functions-overview?WT.mc_id=maximerouiller-blog-marouill) on what is possible.

Now what is a *Durable* Function? With durable, it's all about orchestration. Just like an orchestrator in music leads the musicians, but each musicians is responsible for their own thing. In terms of Durable Functions, an orchestrator is in charge of telling when a function starts and when to wait for it. So what can be done?

One of the cloud design patterns used with Durable Functions is [Fan-Out/Fan-In](https://docs.microsoft.com/azure/azure-functions/durable-functions-cloud-backup?WT.mc_id=maximerouiller-blog-marouill). This allows you to start X number of Functions, collect their results and, when they are all complete, execute an action. Without the Durable Functions extension, you would have to handle the events, the storage, as well as handling every component of a Function's execution. Durable Functions takes all of this and exposes an API does all the job for you.

That is what we're going to use for my scraping of information from GitHub.

# Fan-out in a web scraping scenario

So what does the code for a fan-out orchestrator looks like?

```csharp
[FunctionName("DownloadSamples_Orchestrator")]
public static async Task<string> RunOrchestrator([OrchestrationTrigger] DurableOrchestrationContext context, TraceWriter log)
{
    string runName = context.GetInput<string>();
    var repositories = await context.CallActivityAsync<List<SampleRepository>>("DownloadSamples_GetAllPublicRepositories", null);

    var tasks = new Task<SampleRepository>[repositories.Count];
    for (int i = 0; i < repositories.Count; i++)
    {
        tasks[i] = context.CallActivityAsync<SampleRepository>("DownloadSamples_UpdateRepositoryData", (runName, repositories[i]));
    }
    await Task.WhenAll(tasks);

    return context.InstanceId;
}
```

So let's decompose what we see here. First, I give a name to every execution of the orchestration. Then, I call a function to retrieve the list of all Public Repositories. This, returns about 900 `SampleRepository`. Then, we create an array of Tasks and go through our 900 elements and start a Function for each of them.

Once all of them are done, we wait for all of them to finish and we optionally return the `InstanceId` of the orchestrator.

And that's it. Every function will only be called once except the orchestrator. The function with an `[OrchestrationTrigger]` is always considered an Orchestrator and can be run multiple time.

[`OrchestrationTrigger` behaves widely differently than normal Azure Function.](https://docs.microsoft.com/azure/azure-functions/durable-functions-bindings?WT.mc_id=maximerouiller-blog-marouill#trigger-behavior) **THIS IS IMPORTANT.** Don't execute requests to resources (SQL, Storage, API, etc.) in there. This function will be orchestrating the other functions and for my result set, I've seen it ran 10+ times.

# Taking things up a notch

Ain't it amazing? Now, I have an Orchestrator that will make make 900 calls to the `DownloadSamples_UpdateRepositoryData` activity that will retrieve as much information as possible from the GitHub API. But what if I have multiple orchestrators? What if I have multiple pipelines of data ingestion that I want to make?

You just need one more orchestrator!

```csharp
[FunctionName("MainDownloadOrchestrator_TimerStart")]
public static async Task TimerStart([TimerTrigger("0 0 7 * * *")]TimerInfo myTimer,
    [OrchestrationClient]DurableOrchestrationClient starter,
    TraceWriter log)
{
    string instanceId = await starter.StartNewAsync("MainDownloadOrchestrator", null);
    log.Info($"Started orchestration with ID = '{instanceId}'.");
}

[FunctionName("MainDownloadOrchestrator")]
public static async Task<string> RunOrchestrator(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string runName = await context.CallActivityAsync<string>("CommonActivityFunctions_CreateRun", null);

    var downloadPipelines = new List<Task>();
    downloadPipelines.Add(context.CallSubOrchestratorAsync("DownloadSamples_Orchestrator", runName));
    downloadPipelines.Add(context.CallSubOrchestratorAsync("DownloadSomethingElse_Orchestration", runName));
    await Task.WhenAll(downloadPipelines);

    return context.InstanceId;
}
```

That is a simplified version of my Orchestrator in my code. What does it do? Create a single *run name* that we're going to be using to invoke every other data ingestion pipeline (read: orchestrator).

All those orchestrators are all going to start at 7am every day and since the only common data that we have has been created prior to starting the flow of data ingestion... everything is going to run successfully.

# Why do I want this?

You want this because imagine working on a team with many different processes that needs to be run in parallel. Maybe one team is working on the shipping process, the other on the payment process. With a single orchestrator to manage two sub orchestrators, it makes team collaboration way easier.

In my case, what if someone else want to add the parsing of another API? They can work on their own orchestrator and plug its invocation in my `MainDownloadOrchestrator` Function and we'd be dnoe.

# What's missing?

What you haven't seen in this is mainly rate limiting. GitHub API has a limit of 5000 (at the time of writing) API calls per hour which is more than enough for the use that I make of it.

However, if other teams were to query GitHub some more, I'd need to look into implementing it. I still don't have an elegant solution for this but it's going to be something I'm looking to implement next.

# Try it out

If you want to try it out, Azure Functions comes with a free quota. If you need an account, [create an account for free](https://azure.microsoft.com/free/?WT.mc_id=maximerouiller-blog-marouill).

Also, here's a list of resources to get you started on Azure Functions:

* [Introduction to Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview?WT.mc_id=maximerouiller-blog-marouill)
* [Creating your first function in the Azure Portal](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function?WT.mc_id=maximerouiller-blog-marouill)
* [Running Azure Functions on a Timer Trigger](https://docs.microsoft.com/azure/azure-functions/functions-create-scheduled-function?WT.mc_id=maximerouiller-blog-marouill)
* [Installing the Durable Functions extensions](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-install?WT.mc_id=maximerouiller-blog-marouill)