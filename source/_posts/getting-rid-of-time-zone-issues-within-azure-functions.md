---
title : "Getting rid of Time Zone issues within Azure Functions"
date: 2019-06-14 11:25:00
tags: [azure, serverless,.net]
---

Your client wants to run a database clean up task every day at 2 am. Why? Mostly because it's when there's no traffic on the site and it reduces the amount of risk of someone encountering problems due to maintenance tasks.

Sounds good! So as an engineer, you know that task is periodic and you don't need to create a Virtual Machine to handle this task. Not even an AppService is required. You can go serverless and benefit from those free million monthly executions that they offer! Wow. Free maintenance tasks!

You create a new Function app, write some code that looks something like this.

```csharp
[FunctionName("DatabaseCleanUpFunction")]
public static void Run([TimerTrigger("0 0 2 * * *")]TimerInfo myTimer, ILogger log)
{
    // todo: clean up the database
}
```

Wow! That was easy! You publish this application to the cloud, test it out manually a few times maybe then publish it to production and head back home.

When you get back to the office the next day, you realize that the job has run yes... but at 10 pm. Not 2 am. What happened!?

You've been struck by Time Zones. Oh, that smooth criminal of time.

## Default time zone

The default timezone for Azure Functions is UTC. Since I'm in the Eastern Time Zone, this previous function now runs at 10 pm instead of 2 am. If there were users that were using my application at that time, I could have severely impacted them.

That is not good.

## The Fix

There are two ways to go around this. One, we change our trigger's CRON Expression to represent the time in UTC and keep our documentation updated.

The second way is to tell Azure Functions in which timezone they should interpret the CRON Expression.

For me, a man of the best Coast, it involves setting the `WEBSITE_TIME_ZONE` environment variable to `Eastern Standard Time`. If you are on the lesser Coast, you may need to set yours to `Pacific Standard Time`.

However, let's be honest. We're in a global world. You need [The List™][TheList].

Find your region on that list, set it in the `WEBSITE_TIME_ZONE`, and Azure Function automatically set the correct Time Zone for your CRON expression.

[TheList]: https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc749073(v=ws.10)?WT.mc_id=personal-blog-marouill#time-zones