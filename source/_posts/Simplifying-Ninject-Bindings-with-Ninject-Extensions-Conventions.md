---
title: "Simplifying Ninject Bindings with Ninject.Extensions.Conventions"
date: 2011-02-15 10:56:00
tags: [c#]
---

So in my previous post, we talked about binding interface/abstract class to implementations with Ninject. It was a rather simple task and it allowed us to do the job quite easily.

But of course the example was a simple case and would never ever even look close to a real production project. A real production project might have 3-4 services with at least 2-3 dependencies each that might or might not be the same. This can easily explode when adding new functionalities to a software. Need to send email? Add a dependency on INotifier. Need to access another service elsewhere? Another dependency. This can grow quite big quite fast and it will be a pain to actually maintain all the bindings.

This is where `Ninject.Extensions.Conventions` come and save us some time. 

Let’s take the following code and binding from our previous example: 

```cs
using System;
using Ninject;

namespace ConsoleNinject
{
    class Program
    {
        static void Main(string[] args)
        {
            var kernel = new StandardKernel();

            kernel.Bind<IConsoleOutput>().To<MyConsoleOutput>();

            var output = kernel.Get<IConsoleOutput>();
            output.HelloWorld();

            var service = kernel.Get<Service>();
            service.OutputToConsole();

            Console.ReadLine();
        }
    }

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
}
```

So we only have one line of binding. Now don’t forget that this line will explode to 4, 6, 10, 20 lines as things move forward. We want to simplify this to the most simplest case.

This line can be replaced with the following:

```cs
kernel.Scan( x=>
    {
        x.FromAssembliesMatching("*");
        x.BindWith<DefaultBindingGenerator>();
    });
```

However, running the code after that won’t work. Why? Because the `DefaultBindingGenerator` binds I{Name} to {Name}. Quite simple but it’s extendable.

So we need to change the name of our class `MyConsoleOutput` to `ConsoleOutput` and then everything works! 

There you go! Now we can keep on adding interfaces and implementations without changing the actual binding codes. 

On another note, you might want to change which assemblies are loaded for the conventions since this would load all DLLs in the executing directory.