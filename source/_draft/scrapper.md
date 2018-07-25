---
title : "Cloud Scale API scraping with serverless orchestrators"
date:
tags: [azure,serverless]
---

One of the things I'm working on requires me to retrieve information from multiple sources like the NuGet API and GitHub API. Let's take a concrete example of one of the things I did recently. If you follow me [on Twitter](https://twitter.com/MaximRouiller), you probably already know about it.

There's tons of Samples on the [Azure-Samples](https://github.com/Azure-Samples/) organization on GitHub, and I want to be able to check them out to see which ones are "too old". For example, what about a sample that was last modified 1 month ago? What about 3 months or 6 months? What if the sample is over a year old? Is it relevant?

We need a way to retrieve those 900+ samples and retrieve their last commit date.

# Our scenario

Ever ended up on a sample that should be covering the problem you're having but it just doesn't work? Then, you check the last commit date only to realize that it's been 2 years since the last commit. Finally, you check the packages and realize that they are 3 major versions behind. That sample is almost no good to you.

Well, the sample repositories on the Azure-Samples organization have those exact issues, and it's one of the many problems that I'm trying to solve. However, we first need to be able to retrieve all that information.

# File -> New Project -> Console Application

Our first instinct as programmers is to try to do it once through a console application. It's the minimum amount of complexity. So, I went ahead and started by consulting the [GitHub API documentation](https://developer.github.com/v3/), created a GitHub token and started hunting for the information I was looking for.

I ended up using the [Octokit]() library to do most of my API access. The code to retrieve the last commit date is seen below.

```csharp
var github = new GitHubClient(new ProductHeaderValue("AzureSampleChecker")) { Credentials = new Credentials("<token>") };

var lastCommit = await github.Repository.Commit.GetAll(repositoryId, new ApiOptions { PageSize = 1, PageCount = 1 });
var commit = lastCommit.FirstOrDefault()?.Commit;
var lastCommitDate = commit.Committer.Date;

//snipped: saving to the database
```

# First problem: Running locally

So, everything was running fine but the problem for me at that point was it was just a single console application running locally. I could have easily taken it to containers and be done with it, but I saw another way to solve this. I managed to migrate everything into an Azure Function. The problem now was that it was still just a console application running inside an Azure Functions. Lame.

For starters, once you know all the repositories it's a distributed problem. How many repositories can I hit at once without having any state to correlate? The answer is all of them. But first, I needed to refactor.

# Introducing Durable Functions

So we all know what serverless is. If you need a refresher, you can review the [Azure Functions Overview](https://docs.microsoft.com/azure/azure-functions/functions-overview?WT.mc_id=maximerouiller-blog-marouill) on what is possible.

Now what is a *Durable* Function? With durable, it's all about orchestration. Just like an orchestrator in music leads the musicians, but each musicians is responsible for their own thing. In terms of Durable Functions, an orchestrator is in charge of telling when a function starts and when to wait for it. So what can be done?

## Fan-In/Fan-Out

One of the cloud design patterns I used with Durable Functions is [Fan-Out/Fan-In](https://docs.microsoft.com/azure/azure-functions/durable-functions-cloud-backup?WT.mc_id=maximerouiller-blog-marouill).

![Fan-In/Fan-Out](/posts/files/durablefunctions/fan-out-fan-in.png)

Fanning out means that your orchestrator function (F1) starts as many functions (F2) as necessary in parallel with some initial parameters like the repository I want. Once all those functions have finished executing, we need a way to return the requested data back to our orchestrator (F3). Keep in mind that, all those functions may not be executing on the same server. It is not as simple as a multi-threaded application. It's a multi-threaded, multi-server, highly parallel execution workflow.

Normally, you'd want to aggregate all the content in storage (Azure Storage/S3/local files/etc.) and then, run more code to aggregate them all together. This process is called fan-in whereas you take all the individual result of all functions and group them into a single result. Without Durable Functions, this process would require you to use storage, messaging and maybe even a service bus to coordinate everything into a consistent process.

With Azure Functions, it's as easy as returning an object. All the necessary work of storing the results of individual functions and aggregating them together is done automatically by the SDK.

This is a hard problem. Durable Functions just saved me easily one week of work and more days of testing, debugging, and refining.

## Function Chaining

Another pattern I want to quickly cover is function chaining because it's the easiest and most commonly used one.

![Function Chaining](/posts/files/durablefunctions/function-chaining.png)

Everytime that your orchestrator `await` a function before running another one, you're basically chaining them. Here's the above image in a code example.

```csharp
[FunctionName("FunctionChaining")]
public static async Task RunFunctionChaining([OrchestrationTrigger]) DurableOrchestrationContext context)
{
    await context.CallActivityAsync("F1", null);
    await context.CallActivityAsync("F2", null);
    await context.CallActivityAsync("F3", null);
    await context.CallActivityAsync("F4", null);
}
```

Now that the introduction is complete, let's jump on back to our scenario.

# Fan-out in a web scraping scenario

So what does the code for my samples orchestrator looks like?

```csharp
[FunctionName("DownloadSamples_Orchestrator")]
public static async Task RunOrchestrator([OrchestrationTrigger] DurableOrchestrationContext context, TraceWriter log)
{
    var repositories = await context.CallActivityAsync<List<SampleRepository>>("DownloadSamples_GetAllPublicRepositories", null);

    var tasks = new Task<Samples>[repositories.Count];
    for (int i = 0; i < repositories.Count; i++)
    {
        tasks[i] = context.CallActivityAsync<Samples>("DownloadSamples_UpdateRepositoryData", repositories[i]);
    }
    await Task.WhenAll(tasks);

    var samplesToAdd = tasks.Select(x => x.Result).ToList();
    await context.CallActivityAsync("DownloadSamples_SaveAllToDatabase", samplesToAdd);
}

```

So let's decompose what we see here. First, I call a function to retrieve the list of all Public Repositories available on `Azure-Sampels` and wait for the results before going any further. This, returns about 900 `SampleRepository`. Then, we create an array of Tasks and go through our 900 elements and start a function without an `await` for each of them. We're *fanning out* the execution of those functions to be run and scaled by the cloud automatically. We're not using `await` here otherwise, they would run sequencially instead of in parallel.

Once all of them are initialized, we wait for all of them to finish. Finally, we aggregate the `Samples` object that has been returned by those functions into a list and send it finally to a function that will save them in a database. We've successfully *fanned-in* the results of 100s of functions. No additional code. No other configurations.

Just like that, we've made a complex parallel problem easy.

## Talking about the Orchestrator Function

Functions with the [`OrchestrationTrigger` behaves widely differently than normal Azure Function.](https://docs.microsoft.com/azure/azure-functions/durable-functions-bindings?WT.mc_id=maximerouiller-blog-marouill#trigger-behavior) With that attribute, it becomes an Orchestrator.

Its behavior are extremely different than other Functions. It will be called multiple times at different moment to orchestrate the execution of those functions. It is of the utmost importance that you do not calculate time and don't access external resources (SQL, Storage, API, etc.) in there. This function will be checking the status of those functions we started by re-executing the orchestrator function multiple times.

It's also important to note that every call that you make using `CallActivityAsync` won't be reexecuted twice. Results are cached and will be handled by the Durable Functions SDK. 

# Taking things up a notch

Isn't it amazing? Now, I have an Orchestrator that will make 900 calls to the `DownloadSamples_UpdateRepositoryData` activity that will retrieve as much information as possible from the GitHub API. But what if I have multiple orchestrators? What if I have multiple pipelines of data ingestion that I want to make?

How do I orchestrate the orchestrators? By making another orchestrator of course!

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
    var runId = await context.CallActivityAsync<string>("CommonActivityFunctions_CreateRun", null);

    var downloadPipelines = new List<Task>();
    downloadPipelines.Add(context.CallSubOrchestratorAsync("DownloadSamples_Orchestrator", runId));
    downloadPipelines.Add(context.CallSubOrchestratorAsync("DownloadSomethingElse_Orchestration", runId));
    //todo: add more orchestrators
    await Task.WhenAll(downloadPipelines);

    return context.InstanceId;
}
```

That is a simplified version of my Orchestrator in my code. What does it do? Create a single *run* that we're going to be using to invoke every other data ingestion orchestrator.

All those orchestrators are all going to start at 7am every day at the same time. All those orchestrators are going to run as described before. Only this time, they will also report back to another orchestrator.

Once you start implementing simple workflows, it becomes really easy to create more complex one like the one above.

# Why do I want this?

Imagine working on a team with many different processes that needs to be run in parallel. Maybe one team is working on the shipping process, the other on the payment process. With a single orchestrator to manage two sub orchestrators, it makes team collaboration easier.

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