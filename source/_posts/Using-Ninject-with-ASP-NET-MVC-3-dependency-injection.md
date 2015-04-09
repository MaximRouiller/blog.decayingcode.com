---
title: "Using Ninject with ASP.NET MVC 3 dependency injection"
date: 2011-02-11 12:25:00
tags: [c#,asp.net]
---

Ninject is my favorite container for doing dependency injection. However, it always seemed a bit complicated to use with ASP.NET WebForms.

Things have improved with ASP.NET MVC 1 of course but now, with the release of ASP.NET MVC 3, things have become way easier to do.

Here is what my Global.asax.cs looks like when I want to register an IoC tool for ASP.NET MVC 3:

```cs
protected void Application_Start()
{
    AreaRegistration.RegisterAllAreas();

    RegisterGlobalFilters(GlobalFilters.Filters);
    RegisterRoutes(RouteTable.Routes);
    RegisterDependencyResolver();
}

private void RegisterDependencyResolver()
{
    var kernel = new StandardKernel();

    DependencyResolver.SetResolver(new NinjectDependencyResolver(kernel));
}
```

Well that look easy enough no? Well, to be fair&hellip; **NinjectDependencyResolver** doesn&rsquo;t exist. It&rsquo;s a custom class that I created to allow the easy mapping of the Kernel.

The only thing it needs is the Ninject **StandardKernel**. Once all the mappings are done, we give the kernel to the resolver and let MVC handle the rest.

Here is the code for NinjectDependencyResolver for all of you to use:

```cs
public class NinjectDependencyResolver : IDependencyResolver
{
    private readonly IKernel _kernel;

    public NinjectDependencyResolver(IKernel kernel)
    {
        _kernel = kernel;
    }

    public object GetService(Type serviceType)
    {
        return _kernel.TryGet(serviceType);
    }

    public IEnumerable&lt;object&gt; GetServices(Type serviceType)
    {
        try
        {
            return _kernel.GetAll(serviceType);
        }
        catch (Exception)
        {
            return new List&lt;object&gt;();
        }
    }
}
```