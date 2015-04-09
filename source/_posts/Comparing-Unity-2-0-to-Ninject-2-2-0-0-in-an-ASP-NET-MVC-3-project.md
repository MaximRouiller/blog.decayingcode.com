---
title: "Comparing Unity 2.0 to Ninject 2.2.0.0 in an ASP.NET MVC 3 project"
date: 2011-02-15 11:56:00
tags: [c#, asp.net]
---

# Tools used

*   Ninject 2.2.0.0
*   Unity 2.0
*   Visual Studio 2010  

# Initial premise 

The way to do Dependency Injection in ASP.NET MVC 3 is basically simple compared to previous versions. ASP.NET MVC 3 expose a static class called `DependencyResolver`. This class has one static method called `SetResolver` that takes into parameter an `IDependencyResolver`. This allows you to set a custom resolver that ASP.NET MVC 3 will you to instantiate your controllers with. Really easy stuff. [As you can see, this is a really simple interface to implement](http://msdn.microsoft.com/en-us/library/system.web.mvc.idependencyresolver.aspx).

## 

## Ninject

Ninject (an open-source tool) already comes with one since December 6th 2010\. This allows you to simply set any configured Ninject `StandardKernel` as a parameter to the resolver and give it to the `DependencyResolver`.

## Unity

Unity, however, doesn’t call it a `DependencyResolver`. The class we are looking for is [UnityServiceLocator](http://msdn.microsoft.com/en-us/library/microsoft.practices.unity.unityservicelocator(v=pandp.20).aspx). Although it doesn’t seem quite apparent, it’s actually compatible with the `DependencyResolver` implementation and can be used with it.

# Bindings

## Ninject

Bindings with Ninject (without extensions) are done like this:

```cs
var kernel = new StandardKernel();
kernel.Bind<IHelloWorld>().To<HelloWorld>();
```

## Unity

Bindings with Unity (without extensions) are done like this:

```cs
var container = new UnityContainer();
container.RegisterType<IHelloWorld, HelloWorld>();
```

This shows off how easy it is for both to setup.

# Extensions

## Ninject

There is a lot of extensions available on top of the actual Ninject project. Ninject is basically only caring about the basic bindings and let the extensions to handle the rest. There is extensions for everything. From injection, conventions, message broker, etc. Here is a few that are, for me, the most interesting:

*   [Ninject.Web.Mvc](https://github.com/ninject/ninject.web.mvc)
*   [Ninject.Extensions.Conventions](https://github.com/ninject/ninject.extensions.conventions)
*   [Ninject.Extensions.Interception](https://github.com/ninject/ninject.extensions.interception)

## Unity

There doesn’t seem to be any extensions available through Unity but rather be already included within the library itself. This might not be the best tactic as the releases of Unity are quite far away from each other and waiting for an update might take weeks or even months. Unity seems to be able to handle UnityContainerExtension but there is not much on the web that was developed by the open source community.

# Which one to choose?

At that point, there isn’t much that differentiate both. It’s mainly a matter of taste whether you like on syntax or the other. However, on the extension side… there is a definite edge on the Ninject project.

Ninject project is definitely more active and you can see each and every modifications made in the source control on GitHub.