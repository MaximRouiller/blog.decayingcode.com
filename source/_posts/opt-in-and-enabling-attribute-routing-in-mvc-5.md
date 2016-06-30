---
title: "Opt-in and enabling Attribute Routing in MVC 5"
date: 2016-06-30 10:00
tags: [asp.net, c#, webapi]
---

It happens to the best of us. You are building a new Web API and suddenly, you need an extra routing for a specific method.

First thing that comes into your head? Attribute Routing. Quick and easy solution for this small problem that just popped.

So you write something along those line:

```csharp
public class ProductController
{
    [Route("product/{productId:int}/details")]
    public async Task<HttpResponseMessage> GetProductDetails(int productId)
    {
        // some code here
    }
}
```

Then you go and test your route and you get a 404. It doesn't work. You try different calls, different variation and before you know it... you've spent 15 minutes on the problem.

Why did it fail?

### Enabling Attribute Routing

Attribute Routing is opt-in. You need to enable it in your `Startup.cs` before you can start using it.

```csharp
config.MapHttpAttributeRoutes();
```

If this line is missing, any attribute routing that are defined will simply be ignored by the framework.

### About Opt-In

Many things that were enabled by default are now going to be opt-in at different level. It makes the framework leaner if it doesn't have to go and look for those attribute to start with. In fact, any option in the framework that is opt-in is just another path it doesn't have to evaluate.

It confuse me sometimes that we need to enable things as basic as this but on the other hand, I'm happy.

A framework should not have everything turned on by default. It should allow you to include what you want while pre-activating some sensible defaults that can be turned off in case of need.

ASP.NET MVC is just doing an excellent job at allowing you to customize what you want and what you don't.

Are we too used to having everything enabled by default? What are your toughts on the matter?
