---
title : ".NET/ASP.NET Documentation Update for August 2019"
date: 2019-09-02 12:00:00
tags: [documentation]
---

**tldr; This is a status update on the .NET documentation. If you want me to do more of those (once a month), please let me know in the comments!**

**NOTE: .NET Core 3.0 will be released at [.NET Conf](https://www.dotnetconf.net). That starts on September 23rd 2019. The conference is online and you'll be able to ask questions. Please consider attending!**

Hi everyone!

This is the third month where I post a summary of all .NET-related documentation that were significantly updated during the month of August.

My name is Maxime Rouiller and I'm a Cloud Advocate with Microsoft. For this month, I'm covering three major products:

* .NET, which had ~249 commits, and 4,247 changed files on their [docs repository](https://github.com/dotnet/docs) and ~184 PRs on their [API docs repository](https://github.com/dotnet/dotnet-api-docs)
* ASP.NET, which had ~269 commits, and 1,615 changed files on their [docs repository](https://github.com/aspnet/AspNetCore.Docs)
* NuGet, which had ~136 commits, and 85 changed files on their [docs repository](https://github.com/NuGet/docs.microsoft.com-nuget)

Obviously, that's a lot of changes and I'm to help you find the gold within this tsunami of changes.

So here are all the documentation updates by product with commentary when available!

## Themes this month

* What's new for the Preview 8
* Updating the CLI documentation
* Adding new tab on the hub to better find e-books
* Removing duplicate articles
* Updating C# 8:
  - Nullable types
  - Default interface
  - Reference page

## .NET Core

There was this addition that was made this month:

* [DateTime and DateTimeOffset support in System.Text.Json](https://docs.microsoft.com/dotnet/standard/datetime/system-text-json-support?noop=docs3-reddit-marouill)

Another VERY interesting article that was updated this month is a guidance article on cross-platform targeting! Now with more code sample!

* [Cross-platform targeting for .NET libraries](https://docs.microsoft.com/dotnet/standard/library-guidance/cross-platform-targeting?noop=docs3-reddit-marouill)

The telemetry article has been thoroughly revised and now includes a link for you to see the telemetry data for the past quarter (Q2):

* [.NET Core SDK telemetry](https://docs.microsoft.com/dotnet/core/tools/telemetry?noop=docs3-reddit-marouill)

The .NET Standard support table was updated to include the 2.1 Preview information:

* [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support?noop=docs3-reddit-marouill)

### Dependency Loading

All of the following are brand new .NET Core articles:

* [Default probing](https://docs.microsoft.com/dotnet/core/dependency-loading/default-probing?noop=docs3-reddit-marouill)
* [Managed assembly loading algorithm](https://docs.microsoft.com/dotnet/core/dependency-loading/loading-managed?noop=docs3-reddit-marouill)
* [Satellite assembly loading algorithm](https://docs.microsoft.com/dotnet/core/dependency-loading/loading-resources?noop=docs3-reddit-marouill)
* [Unmanaged library loading algorithm](https://docs.microsoft.com/dotnet/core/dependency-loading/loading-unmanaged?noop=docs3-reddit-marouill)
* [Dependency loading](https://docs.microsoft.com/dotnet/core/dependency-loading/overview?noop=docs3-reddit-marouill)
* [Understanding AssemblyLoadContext](https://docs.microsoft.com/dotnet/core/dependency-loading/understanding-assemblyloadcontext?noop=docs3-reddit-marouill)

### Diagnostics

Brand new articles on the available diagnostic tools within .NET Core. From managed debuggers, logging, tracing, and unit testing:

* [Diagnostics tools overview](https://docs.microsoft.com/dotnet/core/diagnostics/index?noop=docs3-reddit-marouill)
* [Logging and tracing](https://docs.microsoft.com/dotnet/core/diagnostics/logging-tracing?noop=docs3-reddit-marouill)
* [Managed debuggers](https://docs.microsoft.com/dotnet/core/diagnostics/managed-debuggers?noop=docs3-reddit-marouill)

## ML.NET

New tutorial for developing an ONNX model to detect objects in images:

* [Detect images in objects](https://docs.microsoft.com/dotnet/machine-learning/tutorials/object-detection-onnx?noop=docs3-reddit-marouill)

Articles on Model Builder and ML.NET CLI are now in the main table of contents under *Tools* sections:

* [Tools table of contents](https://docs.microsoft.com/dotnet/machine-learning/automate-training-with-model-builder?noop=docs3-reddit-marouill)

There was a small but important bug fix to the ML.NET CLI telemetry article to fix the environment variable name to opt out of telemetry:

* [Telemetry collection by the ML.NET CLI](https://docs.microsoft.com/dotnet/machine-learning/resources/ml-net-cli-telemetry?noop=docs3-reddit-marouill)

## .NET for Apache Spark

A new tutorial was added based on a customer suggestion:

* [Debug a .NET for Apache Spark application on Windows](https://docs.microsoft.com/dotnet/spark/how-to-guides/debug?noop=docs3-reddit-marouill)

## .NET APIs

.NET Core 3.0 Preview 8 was launched on August 14th, so the API reference documentation for .NET Core and .NET Platform Extensions 3.0 was updated. The API documentation was also updated to include a new version of .NET Standard 2.1 Preview.
The documentation team is also working closely with the .NET developer team to add more API documentation for .NET Core 3.0. We reduced the number of undocumented APIs by 1,336 in August.

Do you want to watch what's going on? Follow the [.NET APIs Docs repository](https://github.com/dotnet/dotnet-api-docs/)!

## ASP.NET Core

### Fundamentals

* [Localization Extensibility](https://docs.microsoft.com/aspnet/core/fundamentals/localization-extensibility?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
  - Brand new article!
* [File Providers in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/file-providers?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
  - New sections, worth revisiting
* [High-performance logging with LoggerMessage in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging/loggermessage?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
  - This whole page had many paragraphs added, sample code added, must read.
* [URL Rewriting Middleware in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/url-rewriting?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
  - This article was completely rewritten for 3.0.
* [Options pattern in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/options?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
  - All the samples were updated to 3.0, section added for 2.2
* [Logging in .NET Core and ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
  - Revamped for 3.0.
* [Configuration in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/index?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
  - `Default Configuration` section completely rewritten for 3.0. Lots of sample code has also been cleaned up.
* [App startup in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/startup?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
  - Revamped for 3.0.
* [Make HTTP requests using IHttpClientFactory in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/http-requests?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
  - added how to use `IHttpClientFactory` in a console app, lots of cleanup.

### Performance

* [Cache in-memory in ASP.NET Core](https://docs.microsoft.com/aspnet/core/performance/caching/memory?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
* [Response Caching Middleware in ASP.NET Core](https://docs.microsoft.com/aspnet/core/performance/caching/middleware?view=aspnetcore-3.0&noop=docs3-reddit-marouill)

### Security

* [Hosting ASP.NET Core Images with Docker over HTTPS](https://docs.microsoft.com/aspnet/core/security/docker-https?noop=docs3-reddit-marouill)
* [Authentication samples for ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/authentication/samples?view=aspnetcore-3.0&noop=docs3-reddit-marouill)

### gRPC

* [Troubleshoot gRPC on .NET Core](https://docs.microsoft.com/aspnet/core/grpc/troubleshoot?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
  - brand new article on troubleshooting gRPC issues including a section on macOS, untrusted certificates, and more.
* [gRPC services with ASP.NET Core](https://docs.microsoft.com/aspnet/core/grpc/aspnetcore?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
  - added a whole section on configuring gRPC with Kestrel, HTTP/2, and HTTPS

### SignalR

* [ASP.NET Core SignalR configuration](https://docs.microsoft.com/aspnet/core/signalr/configuration?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
  - Added configuration options for all languages, simplified language in some option description.

### Blazor

Do I need to mention that those are all must reads? 

* [ASP.NET Core Blazor state management](https://docs.microsoft.com/aspnet/core/blazor/state-management?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
  - brand new article on Blazor state management!
* [Handle errors in ASP.NET Core Blazor apps](https://docs.microsoft.com/aspnet/core/blazor/handle-errors?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
  - brand new article on error handling with Blazor!

* [Create and use ASP.NET Core Razor components](https://docs.microsoft.com/aspnet/core/blazor/components?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
  - This page received massive amount of changes with updates from the latest previews including a section on globalization, localization, and ample links to the proper APIs/methods/objects have been added.
* [ASP.NET Core Blazor JavaScript interop](https://docs.microsoft.com/aspnet/core/blazor/javascript-interop?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
  - brand new section on the IJSRuntime w/code sample has been added.

### Razor Pages

* [Migrate from ASP.NET Core 2.2 to 3.0 Preview](https://docs.microsoft.com/aspnet/core/migration/22-to-30?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
  - Updated the migration page for the latest Preview changes.
* [ASP.NET Core Razor SDK](https://docs.microsoft.com/aspnet/core/razor-pages/sdk?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
  - Updated for .NET Core 3.0 latest changes
* [Reusable Razor UI in class libraries with ASP.NET Core](https://docs.microsoft.com/aspnet/core/razor-pages/ui-class?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
  - Added section on Typescript integration, excluding static assets
* [Razor Pages route and app conventions in ASP.NET Core](https://docs.microsoft.com/aspnet/core/razor-pages/razor-pages-conventions?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
  - Updated for the latest changes, added code snippets.
* [Razor Pages unit tests in ASP.NET Core](https://docs.microsoft.com/aspnet/core/test/razor-pages-tests?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
  - Updated samples, completely rewritten for 3.0.

### Razor Pages with EF Core in ASP.NET Core Tutorial

This whole tutorial was updated to 3.0 and got good changes to review. Worth going through again!

* [Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
* [Razor Pages with EF Core in ASP.NET Core - CRUD - 2 of 8](https://docs.microsoft.com/aspnet/core/data/ef-rp/crud?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
* [Razor Pages with EF Core in ASP.NET Core - Sort, Filter, Paging - 3 of 8](https://docs.microsoft.com/aspnet/core/data/ef-rp/sort-filter-page?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
* [Razor Pages with EF Core in ASP.NET Core - Migrations - 4 of 8](https://docs.microsoft.com/aspnet/core/data/ef-rp/migrations?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
* [Razor Pages with EF Core in ASP.NET Core - Data Model - 5 of 8](https://docs.microsoft.com/aspnet/core/data/ef-rp/complex-data-model?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
* [Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8](https://docs.microsoft.com/aspnet/core/data/ef-rp/read-related-data?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
* [Razor Pages with EF Core in ASP.NET Core - Update Related Data - 7 of 8](https://docs.microsoft.com/aspnet/core/data/ef-rp/update-related-data?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
* [Razor Pages with EF Core in ASP.NET Core - Concurrency - 8 of 8](https://docs.microsoft.com/aspnet/core/data/ef-rp/concurrency?view=aspnetcore-3.0&noop=docs3-reddit-marouill)

### Tutorials, HowTo, guides, and others

Two brand new tutorials this month.

* [Tutorial: Call an ASP.NET Core web API with JavaScript](https://docs.microsoft.com/aspnet/core/tutorials/web-api-javascript?noop=docs3-reddit-marouill)
* [Publish an ASP.NET Core app to IIS](https://docs.microsoft.com/aspnet/core/tutorials/publish-to-iis?view=aspnetcore-3.0&noop=docs3-reddit-marouill)

All the following tutorials were updated to .NET Core 3.0.

* [Get started with Swashbuckle and ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/getting-started-with-swashbuckle?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
* [Create a web API with ASP.NET Core and MongoDB](https://docs.microsoft.com/aspnet/core/tutorials/first-mongo-app?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
* [Work with SQL in an ASP.NET Core MVC app](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/working-with-sql?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
* [Add a model to an ASP.NET Core MVC app](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/adding-model?view=aspnetcore-3.0&noop=docs3-reddit-marouill)
* [Add validation to an ASP.NET Core Razor Page](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/validation?view=aspnetcore-3.0&noop=docs3-reddit-marouill)

## NuGet

* [Create a NuGet package using MSBuild](https://docs.microsoft.com/nuget/create-packages/creating-a-package-msbuild?noop=docs3-reddit-marouill)
  - New page added that guides you through the process of creating a NuGet package with only MSBUILD.
* [Multi-targeting for NuGet Packages](https://docs.microsoft.com/nuget/create-packages/Supporting-Multiple-Target-Frameworks?noop=docs3-reddit-marouill)
  - Fixed [issue #1544](https://github.com/NuGet/docs.microsoft.com-nuget/issues/1544) which adds how to create dependency groups.
* [nuget.config File Reference](https://docs.microsoft.com/nuget/reference/nuget-config-file?noop=docs3-reddit-marouill)
  - Added `fallbackPackageFolders` section. Added `packageManagement` settings, fixing [issue #1279](https://github.com/NuGet/docs.microsoft.com-nuget/issues/1279).
* [.nuspec File Reference for NuGet](https://docs.microsoft.com/nuget/reference/nuspec?noop=docs3-reddit-marouill)
  - Tons of smaller adjustement to the specification. Issues fixed: [#1135](https://github.com/NuGet/docs.microsoft.com-nuget/issues/1135), [#1051](https://github.com/NuGet/docs.microsoft.com-nuget/issues/1051), [#1450](https://github.com/NuGet/docs.microsoft.com-nuget/issues/1450), [#1318](https://github.com/NuGet/docs.microsoft.com-nuget/issues/1318), [#1093](https://github.com/NuGet/docs.microsoft.com-nuget/issues/1093).
* [NuGet Package Restore](https://docs.microsoft.com/nuget/consume-packages/Package-Restore?noop=docs3-reddit-marouill)
  - This page was mostly rewritten while keeping the Table Of Content mostly intact. Important additions includes: different way to restore packages using MSBUILD, the .NET CLI, Azure Pipelines, and Azure DevOps Server. 
* [Create a NuGet package using the dotnet CLI](https://docs.microsoft.com/nuget/create-packages/creating-a-package-dotnet-cli?noop=docs3-reddit-marouill)
  - Added `csproj` samples, fixed typos, Removed content that belongs to another page.
