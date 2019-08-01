---
title : ".NET/ASP.NET Documentation Update for July 2019"
date: 2019-08-01 00:00:00
tags: [documentation]
---

# .NET/ASP.NET Documentation Update for July 2019

**tldr; This is a status update on the .NET documentation. If you want me to do more of those (once a month), please let me know in the comments! **

Hi everyone!

If you missed our first post for June, well today I'm posting a summary of all .NET related documentation that were updated significantly during the month of July.

My name is Maxime Rouiller and I'm a Cloud Advocate with Microsoft. For the month of July, I'm covering 3 major products.

* .NET, which had ~248 commits, and 3,331 changed files on their [docs repository](https://github.com/dotnet/docs)
* ASP.NET, which had ~190 commits, and 1,413 changed files on their [docs repository](https://github.com/aspnet/AspNetCore.Docs)
* NuGet, which had ~126 commits, and 133 changed files on their [docs repository](https://github.com/NuGet/docs.microsoft.com-nuget)

Obviously, that's a lot of changes and I'm to help you find the gold within this tsunami of changes.

So here are all the documentation updates by product with commentary when available!

## .NET Core

There were lots of consolidation in the documentation happening. Content that was specific to the .NET Framework documentation that also applies to the .NET Core documentation are being moved to the .NET Guide. Things like Native Interop (say hi to COM), the C# Language Reference

## .NET Architecture

The .NET Architecture e-books content was mixed in with the fundamentals content under the .NET Guide. The team wanted to give a better home for those, so a new landing page was created.

[.NET Application Architecture Guidance](https://docs.microsoft.com/dotnet/architecture?wt.mc_id=docs-reddit-marouill)

### Native Interop

* [Exposing .NET Core Components to COM](https://docs.microsoft.com/dotnet/core/native-interop/expose-components-to-com?wt.mc_id=docs-reddit-marouill)
* [COM Interop in .NET](https://docs.microsoft.com/dotnet/standard/native-interop/cominterop?wt.mc_id=docs-reddit-marouill)

### C\#

!!!  WHAT ARE THOSE? HAVE THEY BEEN REWRITTEN? !!!

* [sizeof operator - C# reference](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/sizeof?wt.mc_id=docs-reddit-marouill)
* [Unmanaged types - C# reference](https://docs.microsoft.com/dotnet/csharp/language-reference/builtin-types/unmanaged-types?wt.mc_id=docs-reddit-marouill)
* [delegate operator - C# reference](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/delegate-operator?wt.mc_id=docs-reddit-marouill)
* [nameof operator - C# reference](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/nameof?wt.mc_id=docs-reddit-marouill)
* [Built-in reference types - C# reference](https://docs.microsoft.com/dotnet/csharp/language-reference/builtin-types/reference-types?wt.mc_id=docs-reddit-marouill)
* [User-defined conversion operators - C# reference](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/user-defined-conversion-operators?wt.mc_id=docs-reddit-marouill)
* [Floating-point numeric types - C# reference](https://docs.microsoft.com/dotnet/csharp/language-reference/builtin-types/floating-point-numeric-types?wt.mc_id=docs-reddit-marouill)
* [Operator overloading - C# reference](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/operator-overloading?wt.mc_id=docs-reddit-marouill)

* [Compiler Error CS0844](https://docs.microsoft.com/dotnet/csharp/misc/cs0844?wt.mc_id=docs-reddit-marouill)
  * What is CS0844?
  > Cannot use local variable 'name' before it is declared. The declaration of the local variable hides the field 'name'.
  * More examples added on how to fix the issue.

### VB.NET

There were tons of documentation that needed examples. Here are a few. To change the language to VB, make sure you pick your favorite language on the language selector at the top of the page left of the "Feedback" button.

* [Handles clause requires a WithEvents variable defined in the containing type or one of its base types](https://docs.microsoft.com/dotnet/visual-basic/language-reference/error-messages/handles-clause-requires-a-withevents-variable-defined?wt.mc_id=docs-reddit-marouill)
* [Best Practices for exceptions - .NET](https://docs.microsoft.com/dotnet/standard/exceptions/best-practices-for-exceptions?wt.mc_id=docs-reddit-marouill)
* [LINQ (Language Integrated Query)](https://docs.microsoft.com/dotnet/standard/using-linq?wt.mc_id=docs-reddit-marouill)

### Tutorials, recommendations, and others

This .NET CLI tutorial was rewritten and went from one page to three.

* [Create an item template for dotnet new - .NET Core CLI](https://docs.microsoft.com/dotnet/core/tutorials/cli-templates-create-item-template?wt.mc_id=docs-reddit-marouill)
* [Create a project template for dotnet new](https://docs.microsoft.com/dotnet/core/tutorials/cli-templates-create-project-template?wt.mc_id=docs-reddit-marouill)
* [Create a template pack for dotnet new](https://docs.microsoft.com/dotnet/core/tutorials/cli-templates-create-template-pack?wt.mc_id=docs-reddit-marouill)

---

* [The .NET Portability Analyzer - .NET](https://docs.microsoft.com/dotnet/standard/analyzers/portability-analyzer?wt.mc_id=docs-reddit-marouill)
  * Portability Analyzer article had more details added in. Things like XLSX format, what to do with missing assemblies, etc. Worth a re-read.
* [Containerize an app with Docker tutorial](https://docs.microsoft.com/dotnet/core/docker/build-container?wt.mc_id=docs-reddit-marouill)
  * Added examples, updated dockerfile `FROM`, added steps on how to build the container

## .NET Core API

.NET Core 3.0 Preview 7 was launched on July 23rd, so the API reference documentation for .NET Core and .NET Platform Extensions 3.0 was updated.
The documentation team is also working closing with the .NET developer team to add more API documentation for .NET Core 3.0. We reduced the number of undocumented APIs by 1,374 in July and this effort will continue through the month of August.

* [.NET Core 3.0 API docs](https://docs.microsoft.com/dotnet/api/?view=netcore-3.0&wt.mc_id=docs-reddit-marouill)

## ASP.NET Core

### gRPC

Documentation keeps improving on gRPC. This time, it's security focused. 

* [Authentication and authorization in gRPC for ASP.NET Core](https://docs.microsoft.com/aspnet/core/grpc/authn-and-authz?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)
* [Security considerations in gRPC for ASP.NET Core](https://docs.microsoft.com/aspnet/core/grpc/security?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)

### Troubleshooting

This doc was consolidated from other pages that handled errors and how to troubleshoot.

* [Troubleshoot ASP.NET Core on Azure App Service and IIS](https://docs.microsoft.com/aspnet/core/test/troubleshoot-azure-iis?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)

### Blazor

* [Create and use ASP.NET Core Razor components](https://docs.microsoft.com/aspnet/core/blazor/components?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)


### Security (Authentication/Authorization/etc.)

Authentication without Identity providers tutorial. Very interesting read!

* [Facebook, Google, and external provider authentication without ASP.NET Core Identity](https://docs.microsoft.com/aspnet/core/security/authentication/social/social-without-identity?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)

Those pages received significant changes.

* [Create an ASP.NET Core app with user data protected by authorization](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data?wt.mc_id=docs-reddit-marouill)
* [Account confirmation and password recovery in ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/authentication/accconfirm?wt.mc_id=docs-reddit-marouill)

### MVC / WebAPI

New features in 3.0 that allows you to use HTTP REPL directly from the CLI.

* [Test web APIs with the HTTP REPL](https://docs.microsoft.com/aspnet/core/web-api/http-repl?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)

### Fundamentals

This CLI global tool is used to help you create areas, controllers, views, etc. for your ASP.NET Core applications. Especially useful if you want to create your own. For the source, look no further than [on GitHub](https://github.com/aspnet/scaffolding).

* [dotnet aspnet-codegenerator command](https://docs.microsoft.com/aspnet/core/fundamentals/tools/dotnet-aspnet-codegenerator?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)

The following are pages that received significant changes.

* [Dependency injection in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)
* [.NET Generic Host](https://docs.microsoft.com/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)

### Host and Deploy

* [Deploy ASP.NET Core apps to Azure App Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/azure-apps/index?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)

### SignalR

* [Authentication and authorization in ASP.NET Core SignalR](https://docs.microsoft.com/aspnet/core/signalr/authn-and-authz?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)

### Performance

This article was co-written with /u/stevejgordon! Have you heard about `ObjectPool`? It prevents objects to be garbage collected and reused instead. It's been in .NET Core forever but this article was written with performance in mind.

* [Object reuse with ObjectPool in ASP.NET Core](https://docs.microsoft.com/aspnet/core/performance/ObjectPool?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)

### Tutorials

New tutorial for ASP.NET Core 3.0 Preview this time with jQuery.

* [Tutorial: Call a web API with jQuery using ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/web-api-jquery?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)

Those Razor Pages tutorials received significant changes due in part to the latest Preview.

* [Add a new field to a Razor Page in ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/new-field?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)
* [Add search to ASP.NET Core Razor Pages](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/search?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)
* [Add validation to an ASP.NET Core Razor Page](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/validation?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)
* [Update the generated pages in an ASP.NET Core app](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/da1?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)
* [Work with a database and ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/sql?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)
* [Add a model to a Razor Pages app in ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/model?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)
* [Scaffolded Razor Pages in ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/page?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)
* [Tutorial: Get started with Razor Pages in ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)

2 more tutorials to receive significant changes are focused on Web API and SignalR

* [Tutorial: Create a web API with ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)
* [Get started with ASP.NET Core SignalR](https://docs.microsoft.com/aspnet/core/tutorials/signalr?view=aspnetcore-3.0&wt.mc_id=docs-reddit-marouill)

## NuGet

Including the 5.2 release notes, there' new pages that has been created within the last month. Take a look to stay up to date!

* [NuGet 5.2 RTM Release Notes](https://docs.microsoft.com/nuget/release-notes/NuGet-5.2-RTM?wt.mc_id=docs-reddit-marouill)
* [Create a NuGet package using the dotnet CLI](https://docs.microsoft.com/nuget/create-packages/creating-a-package-dotnet-cli?wt.mc_id=docs-reddit-marouill)
* [Identify the project format](https://docs.microsoft.com/nuget/resources/check-project-format?wt.mc_id=docs-reddit-marouill)
* [Create packages with COM interop assemblies](https://docs.microsoft.com/nuget/create-packages/author-packages-with-COM-interop-assemblies?wt.mc_id=docs-reddit-marouill)
* [Set a NuGet package type](https://docs.microsoft.com/nuget/create-packages/set-package-type?wt.mc_id=docs-reddit-marouill)

A few of significantly modified pages includes handling nuget accounts, and Package Restore.

* [Individual accounts](https://docs.microsoft.com/nuget/nuget-org/individual-accounts?wt.mc_id=docs-reddit-marouill)
* [NuGet Package Restore](https://docs.microsoft.com/nuget/consume-packages/Package-Restore?wt.mc_id=docs-reddit-marouill)
