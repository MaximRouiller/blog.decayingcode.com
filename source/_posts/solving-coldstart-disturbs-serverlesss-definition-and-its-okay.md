---
title : "Solving Cold-Start disturbs serverless' definition and it's okay"
date: 2019-06-19 10:50:00
tags: [azure, serverless]
---

When you look at Azure Functions, you see it as a great way to achieve elastic scaling of your application. You only pay for what you use, you have a free quota, and they allow you to build entire applications on this model.

The whole reason why Azure Functions is free is due to its linked plan. It's called `Consumption Plan`. While Azure Functions is an application model, the consumption plan is where the **serverless** kicks in. You give us your application, and you care less about the servers.

One of the main selling points of serverless is the ability to scale to 0. It allows you to pay only for what you use and it's a win-win for everyone involved.

That would look a little bit like the image below.

![Ideal Scale Behavior](/posts/files/premium-functions/001.png)

### About Cold Starts

Cold start happens when an application loads for the first time on a server. What happens behind the scene look as follows.

1. The cloud receives a request for your application and starts allocating a server for it
2. The server downloads your application
3. The cloud forwards the request received initially to your application
4. The application stacks load up and initialize what it needs to run the code successfully
5. Your application loads up and starts handling the request.

This workflow happens every time your application either goes from 0 to 1 or when the cloud scales you out.

This whole process is essential as Azure can't keep the servers running all the time without blocking other applications from running on the same servers.

That time between when the request initially arrives and is handled by a server can be longer than 500ms. What if it takes a few seconds? What do you do to solve that problem?

![Scale with cold start](/posts/files/premium-functions/002.png)

### Premium Functions

Azure Premium Functions is the best way to resolve that problem. It breaks the definition of Serverless by the fact that you can't scale to 0. It does, however, offer the elastic scale-out that is required to handle a massive amount of load.

![Elastic Scale-Out](/posts/files/premium-functions/elastic.png)

This minimum of one instance is what makes a night and day difference in terms of improving performance. This single instance already has your application on it; the Azure Functions runtime is ready to handle your application.

Having a permanent instance removes most of the longer steps needed to handle a request. It effectively removes cold-start issues as seen below.

![Scale with One Pre-Warmed Instance](/posts/files/premium-functions/003.png)

When would you use Premium Functions? When your application can't have cold-starts. Not every application require the need for this feature, and I wholeheartedly agree. Keep on using the Consumption Plan if it fits your needs, and the cold-start isn't a problem.

If you are among the clients that can't afford to have cold-start time in your application while still needing the bursting of servers, Premium Function is for you.

### Resources

I've gone very quickly over the essential feature that I think fixes one of the more significant problems with serverless.  More features come with Premium Functions. I've left a link to the docs in case you want to read it all by yourself.

* [Azure Premium Functions](https://docs.microsoft.com/azure/azure-functions/functions-premium-plan?WT.mc_id=maximerouiller-blog-marouill#features)
