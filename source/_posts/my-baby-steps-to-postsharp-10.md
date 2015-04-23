---
title: "My baby steps to PostSharp 1.0"
date: 2009-06-13 23:13:00
tags: []
---

So... you downloaded PostSharp 1.0 and you installed it and are wondering... "What's next?".

Well my friends, let me walk you through the first steps of PostSharp. What could we do that would be simple enough? Hummm... what about writing to a debug window? That sounds simple enough! Let's start. So I created a new Console Application project and I added the reference to PostSharp.Laos and PostSharp.Public. As a requirement, the class must be tagged with "Serializable" attribute and implement **OnMethodBoundaryAspect** (not in all case but let's start small here).

Next, I have a few methods I can override. The two that we are interested in right now is "**OnEnter**" and "**OnExit**". Inside of it, we'll say which method we are entering and which one we are exiting. Here are my Guinea pig classes:

```cs
public class FooBar
{
    [DebugTracer]
    public void DoFoo()
    {
        Debug.WriteLine("Doing Foo");
    }

    [DebugTracer]
    public void DoBar()
    {
        Debug.WriteLine("Doing Bar");
    }
}

[Serializable]
[AttributeUsage(AttributeTargets.Method | AttributeTargets.Property)]
public class DebugTracer : OnMethodBoundaryAspect
{
    public override void OnEntry(MethodExecutionEventArgs eventArgs)
    {
        Debug.WriteLine(string.Format("Entering {0}", eventArgs.Method.Name));
    }

    public override void OnExit(MethodExecutionEventArgs eventArgs)
    {
        Debug.WriteLine(string.Format("Exiting {0}", eventArgs.Method.Name));
    }
}
```

See how simple this is? But... does it work? Let's see the trace of calling each methods:

```text
> Entering DoFoo         
> Doing Foo          
> Exiting DoFoo          
> Entering DoBar          
> Doing Bar          
> Exiting DoBar
```

Isn't that wonderful? Compile execute and enjoy. But... what about the community you say? Of course if the tool is not open source there is probably nothing built around it, right? Wrong!

Here is a few resources for PostSharp that include pre-made attributes that are ready to be used:

*   [ValidationAspect](http://validationaspects.codeplex.com/)
*   [Auto-Implement INotifyPropertyChanged with Aspects](http://thetreeknowseverything.net/2009/01/21/auto-implement-inotifypropertychanged-with-aspects/) (for use with Databindings)
*   [PostSharp4Entlib](http://entlibcontrib.codeplex.com/Wiki/View.aspx?title=PostSharp4EntLib "PostSharp4EntLib") (Using Enterprise Library with PostSharp)
*   [Log4PostSharp](http://code.google.com/p/postsharp-user-plugins/wiki/Log4PostSharp) (log4net with PostSharp)
*   [PostSharp4ViewState](http://www.codeplex.com/PostSharp4ViewState)
*   [DesignByContract](http://code.google.com/p/postsharp-user-plugins/wiki/DesignByContract)
*   [PostSharpAspNet](http://code.google.com/p/postsharp-user-plugins/wiki/PostSharpAspNet) (because ASP.NET use some post-processing and this plugin insert itself in the process to enable PostSharp)
*   [MicroContainer](http://www.codeplex.com/MicroContainer) is a dependency injection container designed for .NET Micro Framework
*   [XPO Automatic Properties](http://www.codeplex.com/xpoautoproperties) is for use with Express Persistent Objects by Developer Express

That was everything I could find. Do you know any others?
