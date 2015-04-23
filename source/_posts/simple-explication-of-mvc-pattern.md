---
title: "Simple explication of the MVC Pattern"
date: 2009-12-08 21:48:00
tags: []
---

Since the last time I wrote a blog post was more than a few months ago, I would like to start by saying that I'm still alive and well. I had changes in my career and my personal life that required some attention and now I'm back on track.

So for those that know me, I was participating to the TechDays 2009 in Montreal and presenting the "Introduction to ASP.NET MVC" session. I will also be presenting the same session in Ottawa (in fact this blog post is written on the way to Ottawa with Eric as my designated driver).

So what is exactly ASP.NET MVC? It's simply the Microsoft's implementation of the MVC pattern that was first described in 1979 by Trygve Reenskaug (see [Model-View-Controller](http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller#History) for the full history).

So in more details, the MVC is the acronym of Model, View and Controller. We will see each component and the advantages of having them separated properly.

#### Model

The model is exactly what you would expect. It's your business logic, your data access layer and whatever else is part of your application logic. This I don't really have to explain. It's where your business logic will sit and therefore should be the most tested part of your application.

The model is not aware of the view or of the controller.

#### View

The view is where sit your presentation layer for your application. In a web framework, this is mostly ASPX pages with logic that is limited to showing the model. This layer is normally really thin and only focused on displaying the model. The logics are mostly limited to encoding, localization, looping (for grids) and such.

The view is not aware of which controller invokes it. The view is only aware of the model to display.

#### Controller

The controller is the coordinator. It will retrieve data from the model and hand it over to the view to display. The controller can also be associated to other cross-cutting concerns such as logging, authorization and performance monitoring (performance counter, timing each operations, etc.).

#### Advantages

Now, why should you have to care about all that? First, there is a clear cutting separation between **WHAT** is displayed to the user and **HOW** you get the information to display. In the example of a web site, it become clearly possible to display different views based on the browser, the device, the capabilities of the device (javascript, css, etc...) and any other information available to you at the moment.

Among the other advantages, it's the ability to test your controller separated from your view. If your model is properly done too (coding against abstraction and not implementations), you will be able to test your controller separated from your model and your view.

#### Disadvantages

Mostly a web pattern than a WinForm pattern. There is currently no serious implementation of the MVC pattern for anything else other than web frameworks. The MVC pattern is hence found in ASP.NET MVC, FubuMVC and other MVC Framework. Thus it limits your choices to the web.

If you take a specific platform like ASP.NET MVC, other disadvantages (that could be seen as advantages) slips in. Mostly, you lose any drag and drop support for server controls. Any grids are now required to be hand-rolled and built manually instead the more usual abstraction offered by the original framework.

#### Conclusions

Since we mostly require to have a more fine grained control over our view, the abstraction offered by the core .NET Framework are normally not extensible/customizable enough for most web designers. Some abstraction might even become unsupported in the future and thus bringing us to a more precise control of our views. The pattern also allow us greater testability than what is normally offered by default in WebForms (Page Controller with Templating Views).

My recommendation will effectively be a "it depend". If an application is already built with WebForms and doesn't have any friction, there is no point in redoing the application completely in MVC. However, for any new greenfield project, I would recommend at least taking a look at ASP.NET MVC.

