---
title : "What's new in VS2017? - Lightweight Solution Load"
date: 2017-03-31 14:00
tags: [visual studio]
---

There's a brand new option in Visual Studio 2017 that many users might overlook. It's called **Lightweight Solution Load**.

While opening solutions in most project is relatively simple, some just simply aren't.

If you take a solution like [roslyn](https://github.com/dotnet/roslyn); this project has around 200 projects. That is massive. To be fair, you don't need to be that demanding to see performance degradation in Visual Studio. Even if Visual Studio 2017 is faster than 2015, huge solutions can still take a considerable amount of time to load.

This is where Lightweight Solution Load comes into play.

### What is lightweight solution load?

Once this option is turned on, Visual Studio will stop pre-loading all projects completely but instead rely on the minimal amount of information needed to have it *functional* in Visual Studio. Files will not be populated until the project is expanded and other dependencies that are not required yet will also not be loaded.

This allows for you to open a solution, expand a project, edit a file, recompile, and be on your way.

### How to turn it on?

There's two ways you can turn it on. Individually for a single solution like so:

![Turning it on for an Individual Solution](/posts/files/lightweight-solution-load/one-solution.png)

Or globally for all future solutions that are going to be loaded:

![Turning it on for All Solutions](/posts/files/lightweight-solution-load/all-solutions.png)

### What is the impact?

Beside awesome performance? There might be Visual Studio features that will just not work unless a project is fully loaded. Please see the list of [known issues](https://www.visualstudio.com/en-us/news/releasenotes/vs2017-relnotes#KILSL) for the current release.

### Alternative

If you do not want to play with this feature but you still find your solution to be too long to load, there's an alternative.

Break up your solutions in different chunks. Most applications can be split into many smaller size solutions. This reduces load time as well as faster compile time in general.

### Are you going to use it?

When new features are introduced, I like to ask people whether it's a feature they would use.

So please leave me a comment and let me know if this feature is going to be used on your projects. How many projects do you normally have in a solution?
