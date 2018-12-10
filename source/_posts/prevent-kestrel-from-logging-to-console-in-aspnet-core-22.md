---
title : "Prevent Kestrel from logging to Console in ASP.NET Core 2.2"
date: 2018-12-10 10:00:00
tags: [.net core]
---

I recently had the need to start a web server from the command line. Authentication, you see, is a complicated process and sometimes requires you to open a browser to complete that process.

Active Directory, requires you to have a *Return Url* which isn't really possible from a command line. Or is it?

With .NET Core, you can easily [setup a web server](https://docs.microsoft.com/aspnet/core/fundamentals/servers/weblistener?view=aspnetcore-1.1#configure-your-aspnet-core-application?WT.mc_id=maximerouiller-blog-marouill) that listens to `localhost`. Active Directory, can be configured to redirect to `localhost`. Problem solved, right?

Not exactly. I'm partial to not outputing anything to useful when creating a CLI tool. I want kestrel to not output anything in the console. We have other ways to make an EXE talk, you see.

So how do you get Kestrel to go silent?

The first solution led me to change how I built my `WebHost` instance by adding `.ConfigureLogging(...)` and `using Microsoft.Extensions.Logging`. It is the perfect solution when you want to tell kestrel to not output anything from individual requests.

However, Kestrel will still output that it started a web server and on which ports it's listening to. Let's remove that too, shall we?

```csharp
WebHost.CreateDefaultBuilder()
  .SuppressStatusMessages(true) // <== this removes the "web server started on port XXXX" message
  .ConfigureLogging((context, logging) =>
  {
      // this removes the logging from all providers (mostly console)
      logging.ClearProviders();
      //snip: providers where I want the logging to happen
  })
  .UseStartup<Startup>()
  .UseKestrel(options =>
  {
      options.ListenLocalhost(Common.Port); // port that is hardcoded somewhere 🤷‍♂️
  });
```

So next time you need Kestrel to take five, you know what to do.