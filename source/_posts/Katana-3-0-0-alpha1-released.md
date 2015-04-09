---
title: "Katana 3.0.0 alpha1 released"
date: 2014-02-21 07:30:00
tags: [owin,katana]
---

The package was released yesterday under the name Microsoft.Owin and the version 3.0.0-alpha1 on NuGet.

The release is actually planned for April as per the [roadmap](http://katanaproject.codeplex.com/wikipage?title=roadmap&amp;referringTitle=Home "roadmap&amp;referringTitle=Home"). What is planned in this released?

*   WS-Federation - Middleware component supporting the WS-Federation protocol for federated identity in an OWIN pipeline.  <li>Remove support for .NET Framework 4.0 in order to exclusively use async await 

But from the [changesets](http://katanaproject.codeplex.com/SourceControl/list/changesets), it appears to be that the first point one delivered.

So what is WS-Federation? It’s a standard for securing web services that has been made by Oracle, Microsoft and way more companies.

For more information, I recommend [this MSDN Library article](http://msdn.microsoft.com/en-us/library/bb498017.aspx "Understanding WS-Federation") for a more thorough understanding.

If you try it, keep me posted on Twitter [@MaximRouiller](https://twitter.com/MaximRouiller).