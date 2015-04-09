---
title: "Community Update 2014-02-17 – #aspnet, #webapi, #owin and some #angularjs"
date: 2014-02-17 16:05:20
tags: [community update]
---

I’m bringing you some old stuff with the Page Life Cycle but it’s a necessary read. If you haven’t read it yet and you are still working with WebForms, I suggest you take a look.

Rick Strahl brings us 2 very interesting post. One for enabling WebAPI Basic Authentication and one for a bug that slipped in recent KB update. So if your WebForms default to MVC login URL, check that out.

On the OWIN side of things, a new release of Security providers brings you a whole lot of new supported OpenID provider and half-ogre brings you a basic pattern to lambda dispatcher for OWIN.

Enjoy!

### ASP.NET

*   [ASP.NET Page Life Cycle Overview (msdn.microsoft.com)](http://msdn.microsoft.com/en-us/library/ms178472(v=vs.100).aspx) – Old one but a goodie. Especially if you are still working with WebForms.
*   [A WebAPI Basic Authentication Authorization Filter - Rick Strahl's Web Log (weblog.west-wind.com)](http://weblog.west-wind.com/posts/2013/Apr/18/A-WebAPI-Basic-Authentication-Authorization-Filter)
*   [Forms Auth loginUrl not working after Windows Update? - Rick Strahl's Web Log (weblog.west-wind.com)](http://weblog.west-wind.com/posts/2014/Feb/15/Forms-Auth-loginUrl-not-working-after-Windows-Update) – This is a bug after a patch to the framework. See how to resolve this issue if you have it. 

### OWIN

*   [NuGet Gallery | Owin.Security.Providers 1.3.0 (www.nuget.org)](http://www.nuget.org/packages/Owin.Security.Providers/) – Additional providers including LinkedIn, Yahoo, Github, Reddit, Steam, etc.
*   [half-ogre/OwinDispatcher · GitHub (github.com)](https://github.com/half-ogre/OwinDispatcher) – Basic pattern to Lambda dispatcher 

### JavaScript

*   [Refactoring the AngularJS code in the RawStack - The Problem Solver (msmvps.com)](http://msmvps.com/blogs/theproblemsolver/archive/2014/02/17/refactoring-the-angularjs-code-in-the-rawstack.aspx)