---
title: "Introduction to Ninject 2.1"
date: 2011-02-11 13:11:14
tags: [c#]
---

# Links

*   [Official Ninject website](http://ninject.org)*   [Ninject on Github](https://github.com/ninject)*   [Ninject download list](https://github.com/ninject/ninject/downloads)  

# What is Ninject?

Ninject is a Inversion of Control (a.k.a IoC) tool that allows you to map Interfaces to Implementations. 

An IoC main job is to instantiate an object that is linked to a requested type. An IoC tool will also control the lifetime of those objects to allow more flexibility in the invocation of them.

# How do I use it?

Well first, you have to download the original version from the Ninject Github site.

There, you are met with many different options that will allow you to match your need. It is important to notice that if you are using a specific Ninject Extension, it requires the same version of Ninject to be used (packaged inside the Extension’s zip file).

For the demo purpose, we’re going to use the .NET 4.0 version of Ninject.

# Prerequisites

Now let’s try to build a simple project. We’ll create a solution containing only a Console application for demo purpose and add the Ninject reference.

[![NinjectConsoleProject](/posts/files/NinjectConsoleProject_thumb.png "NinjectConsoleProject")](/posts/files/NinjectConsoleProject.png)

Then we need an interface and a class implementing that interface to simulate a real-life situation. 

```cs
public interface IConsoleOutput
{
    void HelloWorld();
}

public class MyConsoleOutput : IConsoleOutput
{
    public void HelloWorld()
    {
        Console.WriteLine("Hello world!");
    }
}
```

Once this is implemented, we have everything we need to implement a simple demo for Ninject.

# Ninject Implementation

So in a highly coupled application, we would see a direct invocation of `MyConsoleOutput` and it would end there. However, when that class change or if we want to test that class, it will be impossible to do so without invoking it. By coding directly against an interface or an abstract class, it allows us to decouple the implementation and the contract.

So next, we need to implement the standard ninject container that will hold “Bindings”. Those bindings represent the link between an interface and it’s implementation. Under Ninject, the container is called a Kernel and is represented by the class `StandardKernel`.


```cs
var kernel = new StandardKernel();
```

Then we create the binding between `IConsoleOutput` and `MyConsoleOutput`.


```cs
kernel.Bind<IConsoleOutput>().To<MyConsoleOutput>();
```

Finally, we’ll request an instance of IConsoleOutput and execute it. You should receive a “Hello world” on your console.


```cs
var output = kernel.Get<IConsoleOutput>();
output.HelloWorld();
```

Well that was easy! But what if it’s a dependency on another object? Like this:


```cs
public class Service
{
    private readonly IConsoleOutput _output;

    public Service(IConsoleOutput output)
    {
        _output = output;
    }

    public void OutputToConsole()
    {
        _output.HelloWorld();
    }
}
```

Requesting the Service like this and executing it will lead to the same result.


```cs
var service = kernel.Get<Service>();
service.OutputToConsole();
```

That’s it! 

# Conclusion

So Ninject resolves the dependencies of specific objects. What isn’t shown in this demo is that dependencies on dependencies are also going to be resolved. This allows you to build object graph really easily without having to build a factory or have a huge instantiations call. This allows you to have objects that are loosely coupled and highly testable. 

Of course, Ninject isn’t the only tool that can do that job but so far, I’ve found it to be the easiest.

# The alternatives

*   [Unity](http://unity.codeplex.com/) by Microsoft
*   [StructureMap](http://structuremap.net/structuremap/)
*   [AutoFac](http://code.google.com/p/autofac/)
*   [Castle Windsor](http://stw.castleproject.org/Windsor.MainPage.ashx)