---
title: "OWIN – Introduction to hosts and pipelines"
date: 2013-12-10 09:36:20
tags: [asp.net]
---

### Introduction

OWIN is a new way to build application. With or without the normal IIS server. To retrieve the original description from [OWIN.org](http://owin.org): 
 > OWIN defines a standard interface between .NET web servers and web applications. The goal of the OWIN interface is to decouple server and application, encourage the development of simple modules for .NET web development, and, by being an open standard, stimulate the open source ecosystem of .NET web development tools. 

Four layers are present. The **Application** layer, the **Application Framework** layer, the **Server** layer and finally the **Host** layer.

The **Application** layer is where your code will end-up. The **Application Framework** is where you will find something like WebAPI and ASP.NET. The **Server** Layer will represent what is currently handling resource for your server (think connections pool, thread and what not). As for the **Host**, it’s the technology that will be hosting the whole stack. In this case we have 3 choice. Either it’s IIS, Katana.exe (Microsoft implementation of OWIN) or simply a self-hosted console application that listen on a web port. 

Here’s what it looks like in a nice diagram:

[![image](/posts/files/image_thumb.png "image")](/posts/files/image.png)

### Simply, why should I care?

What is great with OWIN is that you only import what you need. Do you need to serve static files (images, javascript)? Yes? Import the module that handle static files. Do you need Facebook OAuth? Import that too. You have to look at all the features that you application might use and see this as a buffet. You will need many of them but you don’t need everything. What about Google OAuth? Don’t need it? Perfect! It’s not included by default.

The most default application that you can do will literally just listen to a port and return string content without a response type. Here’s what it looks like:

[![Taken from ASP.NET: http://www.asp.net/aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana](/posts/files/image_thumb_1.png "Taken from ASP.NET: http://www.asp.net/aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana")](/posts/files/image_1.png)

[Source](http://www.asp.net/aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana)

This application won’t support login, WebAPI, cookies or even directory browsing. It only returns “Hello, world.” for each and every request. This will lead to very low-memory processes and a highly specialized task of returning “Hello, world.”. However, since we are not writing so simple app, we will need to add support for Web API, routing and maybe even authentication. How do we do this? OWIN defines a way that to specify all this. It’s called the Middleware pipeline. 

### The OWIN Middleware Pipeline

So let’s take the base application. We created a truly empty web application with Visual Studio 2013 and we added the OWIN startup class as described above. How do we add WebAPI in here?

First, we add the package [Microsoft.AspNet.WebApi.Owin](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Owin) to our web project. Then we are going to add before what we already have the proper configuration to use WebAPI 2.

```cs
var routes = new HttpRouteCollection();
routes.MapHttpRoute("Default", "{controller}/{id}", new {id = RouteParameter.Optional});
app.UseWebApi(new HttpConfiguration(routes));
```

With this, we the equivalent of a default WebAPI route. Let’s add a default controller to our system by using “Add New Scaffolded Item” and Empty Web API 2 Controller.

Let’s make a default method to return some random data:

```cs
public class OwinController : ApiController
{
    public OwinItem[] GetAll()
    {
        return new[]
        {
            new OwinItem{Id = 1, Name = "First of them"},
            new OwinItem{Id = 2, Name = "Another one of them"},
        };
    }
}

public class OwinItem
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```

Hitting our URL from the default URL will return the default Hello World. However, hitting your WebAPI with /owin instead will return our fake data and will answer with XML/JSON depending on your HTTP Headers.

Isn’t that cool? We only imported what we needed for that project and nothing more. If you are looking at the Nuget feed for “owin” ([like here](http://www.nuget.org/packages?q=owin)) you will see that a lot of packages has been created. They are all useable by the same patterns. You can basically build your application up from scratch by buying only what you need.

### Pipeline Order

What happens if we invert the UseWebAPI and the Run(…) command? If you try it, you’ll see that nothing else register to WebAPI anymore. The Run element will actually catch everything else that is not handled by a middleware (WebAPI). The Run at the end might be useful if you want to catch any potential request that goes through the .NET pipeline and that isn’t catched.

### OWIN Hosts

Many hosts are available. As of right now, you should still use IIS. However, Katana.exe can be used to host your website or you can simply self-host it within a Console application. As more hosts are being supported, we’ll see greater support for Mono in those scenario. 

### Why should I use OWIN?

Well first there is a lot of cleaning that has been done around which DLLs are actually needed to run a web project. IIS is slowly being geared toward greater support for OWIN with the [IIS Host](http://www.nuget.org/packages/Microsoft.Owin.Host.IIS/0.1.1-pre) instead of the SystemWeb host. As things are moving forward, you will see more and more OWIN development coming in to Visual Studio and your everyday project. OWIN will allow us to build great web software with only the things that we need.

No more will we have to get stuffed with useless DLLs and be stuck by what Microsoft provides out of the box. As things are moving forward, we’ll be able to easily swap parts of the framework so that we can respond quickly to ever changing web. OWIN by itself isn’t meeting all of its promises yet as for what is currently implemented. But as Microsoft and the community moves forward and continue to build great middleware for OWIN, we’ll get access to even more features and be even more nimble than we could ever be before.

I’ll leave you with a list of links that I think you should read. 

### Links to read:

[http://owin.org/](http://owin.org/)

[http://www.asp.net/vnext/overview/owin-and-katana](http://www.asp.net/vnext/overview/owin-and-katana)

[http://www.asp.net/aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana](http://www.asp.net/aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana)

[http://msdn.microsoft.com/en-us/magazine/dn451439.aspx](http://msdn.microsoft.com/en-us/magazine/dn451439.aspx)
