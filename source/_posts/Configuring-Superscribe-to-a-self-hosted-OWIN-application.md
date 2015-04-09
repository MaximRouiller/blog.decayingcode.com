---
title: "Configuring Superscribe to a self-hosted OWIN application"
date: 2014-05-29 17:17:50
tags: [owin,superscribe]
---

We’ll start from my [previous post](http://blog.decayingcode.com/post/creating-a-self-hosted-owin-application) with a single console application with a self-hosted OWIN instance.

The goal here is to provide a routing system so that we can route our application in different section. I could use something like WebAPI but the routing and the application itself are tightly linked.

I’m going to use a nice tool called [Superscribe](http://www.superscribe.org) that allow to do routing. It’s Graph based routing but it should be simple enough for us to hook it up and create routes.

### Installing Superscribe

Well, we’ll open up the Package Manager Console again and run the following command:

```ps
Install-Package Superscribe.Owin
```

This should install all the proper dependencies to have our routing going.

Modifying our Startup.cs to include Superscribe

First thing first, let’s get rid of this silly WelcomePage we created in the previous post. Boom. Gone.

Let’s create some basic structure to handle our routes.

```cs
using Microsoft.Owin;
using Owin;
using Superscribe.Owin.Engine;
using Superscribe.Owin.Extensions;

[assembly: OwinStartup(typeof(MySelfHostedApplication.Startup))]

namespace MySelfHostedApplication
{
    public class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            var routes = CreateRoutes();
            app.UseSuperscribeRouter(routes)
                .UseSuperscribeHandler(routes);
        }

        public IOwinRouteEngine CreateRoutes()
        {
            var routeEngine = OwinRouteEngineFactory.Create();
            return routeEngine;
        }
    }
}
```

So this code basically create all the necessary plumbing for Superscribe to handle our requests.

### Creating our routes

So we now have a route engine to work with. So let’s first create a handler for the default “/” URL.

We’ll also create a route for “/welcome” to use our default WelcomePage that we had earlier (just for demo purpose).

We’ll also create a route for “/Home” that will return a plain text (for the moment).

Here’s what it looks like:

```cs
using Microsoft.Owin;
using Owin;
using Superscribe.Models;
using Superscribe.Owin.Engine;
using Superscribe.Owin.Extensions;

[assembly: OwinStartup(typeof(MySelfHostedApplication.Startup))]

namespace MySelfHostedApplication
{
    public class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            var routes = CreateRoutes();
            app.UseSuperscribeRouter(routes)
                .UseSuperscribeHandler(routes);
        }

        public IOwinRouteEngine CreateRoutes()
        {
            var routeEngine = OwinRouteEngineFactory.Create();
            routeEngine.Base.FinalFunctions.Add(new FinalFunction("GET", o => "Hello world"));
            routeEngine.Pipeline("welcome").UseWelcomePage();
            routeEngine.Pipeline("Home").Use((context, func) =>
            {
                context.Response.ContentType = "text/plain";
                return context.Response.WriteAsync("This is the home page");
            });

            return routeEngine;
        }
    }
}
```

That was simple. 

Basically, we build a simple pipeline to an OWIN middleware.

What about [NancyFX](http://nancyfx.org/) with Superscribe after?