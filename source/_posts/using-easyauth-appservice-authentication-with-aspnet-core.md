---
title : "Using EasyAuth (AppService Authentication) with ASP.NET Core"
date: 2019-07-09 13:15:00
tags: [azure, asp.net core, .net core, security]
---

There's this cool feature in Azure AppService that I love. It's called EasyAuth although it may not use that name anymore.

When you are creating a project and want to throw in some quick authentication Single-Sign-On (SSO for short) is a great way to throw the authentication problem at someone else while you keep on working on delivering value.

Of course, you can get a clear understanding of [how it works][HowEasyAuthWorks], but I think I can summarize quite quickly.

EasyAuth works by intercepting the authentication requests (`/.auth/*`) or when authenticated, fills in the user context within your application. That's the 5-second pitch.

Now, the [.NET Framework application lifecycle](https://support.microsoft.com/help/307985/info-asp-net-http-modules-and-http-handlers-overview?WT.mc_id=personal-blog-marouill) allowed tons of stuff to happen when you added an `HttpModule` in your application. You had access to everything and the kitchen sink.

.NET Core, on the other hand, removed the concept of all-powerful modules and instead introduced [Middlewares](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/?view=aspnetcore-3.0&WT.mc_id=personal-blog-marouill). Instead of relying on a fixed set of events happening in a pipeline, we could expand the pipeline as our application needed it.

I'm not going to go into details on [how to port HttpModules and Handlers](https://docs.microsoft.com/aspnet/core/migration/http-modules?view=aspnetcore-3.0&WT.mc_id=personal-blog-marouill) but let's assume that they are widely different.

One of the many differences is that `HttpModules` could be set within a `web.config` file and that config file could be defined at the machine level. That is not possible with Middlewares. At least, not yet.

## Why does it matter?

So with all those changes, why did it matter for EasyAuth? Well, the application programming model changed quite a lot, and the things that worked with the .NET Framework stopped working with .NET Core.

I'm sure there's a solution on the way from Microsoft but a client I met encountered the problem, and I wanted to solve the problem.

## Solving the issue

So, after [understanding how EasyAuth worked][HowEasyAuthWorks], I've set on to create a [repository](https://github.com/MaximRouiller/MaximeRouiller.Azure.AppService.EasyAuth) as well as a [NuGet package](https://www.nuget.org/packages/MaximeRouiller.Azure.AppService.EasyAuth/).

What I was fixed to do was relay the captured identity and claims into the .NET Core authentication pipeline. I'm not doing anything else.

## Installing the solution

The first step is to install the [NuGet package](https://www.nuget.org/packages/MaximeRouiller.Azure.AppService.EasyAuth/) using your method of choice. Then, adding an `[Authorize(AuthenticationSchemes = "EasyAuth")]` to your controller.

Finally, adding the following lines of code to your `Startup.cs` file.

```csharp
using MaximeRouiller.Azure.AppService.EasyAuth
// ...
public void ConfigureServices(IServiceCollection services)
{
    //... rest of the file
    services.AddAuthentication().AddEasyAuthAuthentication((o) => { });
}
```

That's it. If your controller has an `[Authorize]` attribute, the credentials are going to automatically start populating the `User.Identity` of your MVC controller.

## Question

Should I go farther? Would you like to see this integrated within a supported Microsoft package? Reach out to me directly on [Twitter](https://twitter.com/MaximRouiller) or the [many other ways available](https://developer.microsoft.com/en-us/advocates/maxime-rouiller).

[HowEasyAuthWorks]: https://docs.microsoft.com/azure/app-service/overview-authentication-authorization?WT.mc_id=personal-blog-marouill#how-it-works
