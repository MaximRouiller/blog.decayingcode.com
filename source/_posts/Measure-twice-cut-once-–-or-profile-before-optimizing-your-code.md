---
title: "Measure twice, cut once – or profile before optimizing your code"
date: 2014-03-22 16:00:00
tags: [tool,optimization]
---

Too often we see smart code. I’m not talking about a trademarked product or something science fiction here. I’m talking about code that was written with the goal of making it faster or asynchronous. By smarter I mean, too smart for its own needs. The code should be as simple as it is required to maintain it efficiently and so that other developer can maintain it.

But as the application is being developed, at some point, we realize that it’s slow. So we run the debugger a few times, identify the module from which it’s coming from, assume it’s a certain piece of code and… we optimize.

What did we just optimize? Was it the real problem? Of course since it’s gone now! But right now, the code is unreadable. You’ve added caching, async loading, static dictionary and what not. It’s 20 times faster than it was before! Of course it’s better!

What we don’t realize however is that, we patched the problem. We put a Band-Aid on an open profusely bleeding wound. It will work for now but then you’ll have other problems related to the way you resolved your problem. Caching? Your cache doesn’t invalidate properly… Async? Partial data or 100% CPU every time the task is executed… Dictionary? Thread locking and exception for already existing keys… etc.. etc.. All those problems will gnaw at your time and for what? For some performance issue that you patched by dropping quick hacks on them.

## What should we do?

### Measure. 

Your application was slow. You saw it in your browser. It took forever to load. Each page load were slow as hell. Is it coming from your application startup code? From which layer?

Most of the developer I’ve worked with will normally comment out code and try to reproduce the speed issue. Did it work? No? Comment out some more. Rinse and repeat until the performance issue is resolved. Once they found the source of the problem, they “optimize” or fix this block of code.&nbsp; Most of the time, it might be a “WHERE” on a SQL query that is missing. Sometimes however, I see train wrecks.

The best way to know if your code is slow and what part of your code is slow is to use a profiler. There’s many kind of profiler but I’m going to talk about 2 kinds. Performance profiler and Database profiler. 

### Measuring performance

Here are some tools that most .NET Developers use: [dotTrace](http://www.jetbrains.com/profiler/) ($), [ANTS Performance Profiler](http://www.red-gate.com/products/dotnet-development/ants-performance-profiler/) ($), [slimtune](https://code.google.com/p/slimtune/) (open source)

Those tools will measure the time spent on each methods in your code. They will allow you to home in on code that is invoked too often or few times but that takes an eternity to answer (we’re talking seconds here). This will allow you to find the hotspots in your code. It will tell you where your time should be spent rather than modifying your code to find problems. Maybe your loading huge amount of items in memory and it’s slowing down your app. Maybe an external resource is slow…. whatever it is… those tools will find it.

But what if your code is working fast but its your ORM that is slow?

### Measuring database

Here are some tools that most .NET Developers use: [Hibernating Rhinos profilers for Entity Framework, NHibernate and more](http://www.hibernatingrhinos.com/Products) ($), [MiniProfiler](http://miniprofiler.com/) (open source)

When you realise that your problem is part of your database requests, it’s where you start using database profilers to find your problems. Maybe a badly configured ORM sends SELECT * requests to a table with millions of row while you only need one of them? What is the scenario… knowing what is slow when accessing the database could increase the performance of your application exponentially. 

### Optimizing

Once you are at that step, you know exactly what is slow and what to do to fix it. This is actually the easiest step since you already spent a lot of time understanding the problem. Okay, maybe I’m exaggerating a little but we get the point. Once you know what is the problem, it becomes a lot easier to fix it. 

## Conclusion

Measure first. Optimize. Measure again. If what you did didn’t improve the running time of your application, you only peddled code around. If you gained 2% gain in speed while making the code unreadable to anyone but yourself, you’ve only muddled the water for little to no gain.

Measuring is pretty much the main recommendation of this article. You don’t measure before optimizing, somewhere in the world a kitten dies.

Stop killing kittens.

* * *

#### Related articles

[http://alexandrebrisebois.wordpress.com/2013/08/29/friends-dont-let-friends-use-lazy-loading-on-windows-azure/](http://alexandrebrisebois.wordpress.com/2013/08/29/friends-dont-let-friends-use-lazy-loading-on-windows-azure/)