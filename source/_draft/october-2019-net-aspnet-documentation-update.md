---
title : "October 2019 .NET/ASP.NET Documentation Update"
date: 
tags: [documentation]
---

<!-- docsexp5_personal-blog-marouill -->

**TLDR; This is a status update on the .NET documentation. If you want me to do more of those (once a month), please let me know in the comments!**

**Comment: If you have suggestions, please let me know in the comments. Any product feedback will be forwarded to the proper product team.**

**Question: Should I add Entity Framework in this update as well? Let me know in the comments.**

Hi everyone!

So the .NET Core 3.0 has released, things have settled down a bit and we're ready to start the fifth month of this .NET-related documentation update. This update covers everything that happened since October 1st through November 5th.

My name is Maxime Rouiller and I'm a Cloud Advocate with Microsoft. For this month, I'm covering three major products:

* .NET, which had ~575 commits, and 4,599 changed files on their [docs repository](https://github.com/dotnet/docs) and ~124 PRs on their [API docs repository](https://github.com/dotnet/dotnet-api-docs)
* ASP.NET, which had ~288 commits, and 1,317 changed files on their [docs repository](https://github.com/aspnet/AspNetCore.Docs)
* NuGet, which had ~32 commits, and 26 changed files on their [docs repository](https://github.com/NuGet/docs.microsoft.com-nuget)

As with last month, here's the legend that I will use to mark certain article with certain level of important. Much more useful than writing a blurb beside each one. I know, we're not Twitter. Dare I say I'm using emojis for good? \<insert gasp\>

## Legend

* 📢: Major/Main article that everyone will want to read
* 💥: Important/Must read.
* ✨: Brand new page

Note: It's not because a page doesn't have an icon that it is not important. Everything here is either brand new or significantly modified.

## Themes this month

* CLI updates to .NET Core
* `docsfx.json` plumbing work
* C# 8.0 updates
* C# 8.0 spec updates

## .NET Fundamentals

* ✨ [Reference assemblies](https://docs.microsoft.com/dotnet/standard/assembly/reference-assemblies?wt.mc_id=)
* ✨💥 [I/O pipelines](https://docs.microsoft.com/dotnet/standard/io/pipelines?wt.mc_id=)
* [.NET Documentation homepage](https://docs.microsoft.com/dotnet/index?wt.mc_id=)
  - New tab that lists all the *Architecture* e-books

## .NET Core

### Tools updates for 3.0

* [dotnet run command](https://docs.microsoft.com/dotnet/core/tools/dotnet-run?wt.mc_id=)
* [dotnet sln command](https://docs.microsoft.com/dotnet/core/tools/dotnet-sln?wt.mc_id=)

### Compatibility

* ✨ [Breaking changes, version 3.0 Preview 9 to 3.0 RC1](https://docs.microsoft.com/dotnet/core/compatibility/3.0.9-3.0rc1?wt.mc_id=)
* ✨ [ASP.NET Core breaking changes](https://docs.microsoft.com/dotnet/core/compatibility/aspnetcore?wt.mc_id=)
* ✨ [Networking breaking changes](https://docs.microsoft.com/dotnet/core/compatibility/networking?wt.mc_id=)
* [Breaking changes, version 2.2 to 3.0](https://docs.microsoft.com/dotnet/core/compatibility/2.2-3.0?wt.mc_id=)

### Diagnostics CLI tools

* ✨ [dotnet-counters](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters?wt.mc_id=)
* ✨ [dotnet-dump](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-dump?wt.mc_id=)
* ✨ [dotnet-trace](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-trace?wt.mc_id=)

### C# articles

* ✨[Tutorial: Mix in functionality when creating classes using interfaces with default interface methods](https://docs.microsoft.com/dotnet/csharp/tutorials/mixins-with-default-interface-methods)
* [Write safe and efficient C# code](https://docs.microsoft.com/dotnet/csharp/write-safe-efficient-code?wt.mc_id=)

### C# Language Reference

* ✨ [Nullable value types - C# reference](https://docs.microsoft.com/dotnet/csharp/language-reference/builtin-types/nullable-value-types?wt.mc_id=)
* ✨ [Built-in numeric conversions - C# reference](https://docs.microsoft.com/dotnet/csharp/language-reference/builtin-types/numeric-conversions?wt.mc_id=)
* ✨ [! (null-forgiving) operator - C# reference](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/null-forgiving?wt.mc_id=)
* [readonly keyword - C# Reference](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/readonly?wt.mc_id=)

### Visual Basic

* ✨ [Cannot convert anonymous type to an expression tree because a property of the type is used to initialize another property](https://docs.microsoft.com/dotnet/visual-basic/language-reference/error-messages/bc36548?wt.mc_id=)
* ✨ [NameOf operator - Visual Basic](https://docs.microsoft.com/dotnet/visual-basic/language-reference/operators/nameof?wt.mc_id=)
* ['AddressOf' operand must be the name of a method (without parentheses)](https://docs.microsoft.com/dotnet/visual-basic/language-reference/error-messages/bc30577?wt.mc_id=)

### .NET APIs

* Updated API browser for [.NET Core 3.0](https://docs.microsoft.com/dotnet/api/?view=netcore-3.0&wt.mc_id=), [.NET Standard 2.1](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.1&wt.mc_id=), and [ASP.NET Core 3.0](https://docs.microsoft.com/dotnet/api/?view=aspnetcore-3.0&wt.mc_id=) RTM version.
* Microsoft.Extensions APIs were moved from ASP.NET Core to .NET Platform Extensions.

### ML.NET

* ✨ [Load training data for Model Builder](https://docs.microsoft.com/dotnet/machine-learning/how-to-guides/load-data-model-builder?wt.mc_id=)
* ✨ [ML.NET resources](https://docs.microsoft.com/dotnet/machine-learning/resources/index?wt.mc_id=)
* ✨ ['Tutorial: Classify health violations with Model Builder'](https://docs.microsoft.com/dotnet/machine-learning/tutorials/health-violation-classification-model-builder?wt.mc_id=)
* ✨ ['Tutorial: Automated visual inspection using transfer learning'](https://docs.microsoft.com/dotnet/machine-learning/tutorials/image-classification-api-transfer-learning?wt.mc_id=)
* ✨ ['Tutorial: Forecast bike rental demand - time series'](https://docs.microsoft.com/dotnet/machine-learning/tutorials/time-series-demand-forecasting?wt.mc_id=)

### .NET for Apache Spark

* ✨ [What is Apache Spark?](https://docs.microsoft.com/dotnet/spark/what-is-spark?wt.mc_id=)
* [What is .NET for Apache Spark?](https://docs.microsoft.com/dotnet/spark/what-is-apache-spark-dotnet?wt.mc_id=)
* [Deploy a .NET for Apache Spark application to Databricks](https://docs.microsoft.com/dotnet/spark/tutorials/databricks-deployment?wt.mc_id=)
* [Get started with .NET for Apache Spark](https://docs.microsoft.com/dotnet/spark/tutorials/get-started?wt.mc_id=)
* [Deploy a .NET for Apache Spark application to Azure HDInsight](https://docs.microsoft.com/dotnet/spark/tutorials/hdinsight-deployment?wt.mc_id=)

### Blazor for ASP.NET Web Forms developers e-book

* [Modules, handlers, and middleware](https://docs.microsoft.com/dotnet/architecture/blazor-for-web-forms-developers/middleware?wt.mc_id=)

## ASP.NET Core

### gRPC

* ✨ [Manage Protobuf references with dotnet-grpc](https://docs.microsoft.com/aspnet/core/grpc/dotnet-grpc?wt.mc_id=)
* [Authentication and authorization in gRPC for ASP.NET Core](https://docs.microsoft.com/aspnet/core/grpc/authn-and-authz?wt.mc_id=)

### Blazor

* [Create and use ASP.NET Core Razor components](https://docs.microsoft.com/aspnet/core/blazor/components?wt.mc_id=)
* [Get started with ASP.NET Core Blazor](https://docs.microsoft.com/aspnet/core/blazor/get-started?wt.mc_id=)
* [ASP.NET Core Blazor hosting models](https://docs.microsoft.com/aspnet/core/blazor/hosting-models?wt.mc_id=)

### Fundamentals

* [Use hosting startup assemblies in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/host/platform-specific-configuration?wt.mc_id=)
* [ASP.NET Core Web Host](https://docs.microsoft.com/aspnet/core/fundamentals/host/web-host?wt.mc_id=)
* [Make HTTP requests using IHttpClientFactory in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/http-requests?wt.mc_id=)
* [Globalization and localization in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/localization?wt.mc_id=)
* [Routing in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/routing?wt.mc_id=)
* [Kestrel web server implementation in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel?wt.mc_id=)

### Host and deploy

* [Host and deploy ASP.NET Core Blazor WebAssembly](https://docs.microsoft.com/aspnet/core/host-and-deploy/blazor/webassembly?wt.mc_id=)
* [Docker images for ASP.NET Core](https://docs.microsoft.com/aspnet/core/host-and-deploy/docker/building-net-docker-images?wt.mc_id=)
* [Visual Studio publish profiles for ASP.NET Core app deployment](https://docs.microsoft.com/aspnet/core/host-and-deploy/visual-studio-publish-profiles?wt.mc_id=)

### Migration

* [Migrate from ASP.NET Core 2.2 to 3.0](https://docs.microsoft.com/aspnet/core/migration/22-to-30?wt.mc_id=)

### MVC and Web API

* [Upload files in ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads?wt.mc_id=)
* [Integration tests in ASP.NET Core](https://docs.microsoft.com/aspnet/core/test/integration-tests?wt.mc_id=)
* [Tutorial: Create a web API with ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api?wt.mc_id=)
* [Test web APIs with the HTTP REPL](https://docs.microsoft.com/aspnet/core/web-api/http-repl?wt.mc_id=)
* [JsonPatch in ASP.NET Core web API](https://docs.microsoft.com/aspnet/core/web-api/jsonpatch?wt.mc_id=)

### Performance

* [ASP.NET Core Performance Best Practices](https://docs.microsoft.com/aspnet/core/performance/performance-best-practices?wt.mc_id=)

### Razor Pages

* [Razor Pages route and app conventions in ASP.NET Core](https://docs.microsoft.com/aspnet/core/razor-pages/razor-pages-conventions?wt.mc_id=)

### Security

* [Introduction to Identity on ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/authentication/identity?wt.mc_id=)
* [Scaffold Identity in ASP.NET Core projects](https://docs.microsoft.com/aspnet/core/security/authentication/scaffold-identity?wt.mc_id=)
* [Persist additional claims and tokens from external providers in ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/authentication/social/additional-claims?wt.mc_id=)
* [Facebook, Google, and external provider authentication without ASP.NET Core Identity](https://docs.microsoft.com/aspnet/core/security/authentication/social/social-without-identity?wt.mc_id=)
* [Claims-based authorization in ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/authorization/claims?wt.mc_id=)

### SignalR

* [ASP.NET Core SignalR configuration](https://docs.microsoft.com/aspnet/core/signalr/configuration?wt.mc_id=)
* [Use MessagePack Hub Protocol in SignalR for ASP.NET Core](https://docs.microsoft.com/aspnet/core/signalr/messagepackhubprotocol?wt.mc_id=)
* [Get started with ASP.NET Core SignalR](https://docs.microsoft.com/aspnet/core/tutorials/signalr?wt.mc_id=)

## NuGet

* [NuGet 5.3 Release Notes](https://docs.microsoft.com/nuget/release-notes/NuGet-5.3?wt.mc_id=)
