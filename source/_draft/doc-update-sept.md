---
title : ""
date: 
tags: [documentation]
---

**tldr; This is a status update on the .NET documentation. If you want me to do more of those (once a month), please let me know in the comments!**

**Comment: If you have suggestions, please let me know in the comments. Any product feedback will be forwarded to the proper product team.**

Hi everyone!

This is the fourth month where I post a summary of all .NET-related documentation that were significantly updated. This month covers all changes from September 1st to September 27th.

My name is Maxime Rouiller and I'm a Cloud Advocate with Microsoft. For this month, I'm covering three major products:

* .NET, which had ~474 commits, and 3,492 changed files on their [docs repository](https://github.com/dotnet/docs) and ~96 PRs on their [API docs repository](https://github.com/dotnet/dotnet-api-docs)
* ASP.NET, which had ~282 commits, and 1,102 changed files on their [docs repository](https://github.com/aspnet/AspNetCore.Docs)
* NuGet, which had ~109 commits, and 45 changed files on their [docs repository](https://github.com/NuGet/docs.microsoft.com-nuget)

For this month, I'll be trying something new. I will not be leaving a comment beside each article and will instead use a Legend with emojis (I know, it's Reddit. What am I doing!?). Thing is, it's easier to show you what's what than explain it. Where relevant, I will leave notes.

## Legend

* 📢: Major/Main article that every one will want to read
* 💥: Important/Must read.
* ✨: Brand new page

Note: It's not because a page doesn't have an icon that it is not important. Everything here is either brand new or significantly modified.

## Themes this month

### .NET Core 3

* What's new for Preview 9 and GA release
* JSON serialization
* CLI updates
* Migration docs for 2.2 and 3.0

### Managed Languages

* C# 8.0 Updates
* C# 8.0 Spec updates

## .NET Core

* 💥 [Prerequisites for .NET Core on Windows](https://docs.microsoft.com/dotnet/core/windows-prerequisites?wt.mc_id=)
* 💥 [Breaking changes, version 3.0 Preview 9 to 3.0 - .NET Core](https://docs.microsoft.com/dotnet/core/compatibility/3.0.9-3.0?wt.mc_id=)

### eBook - Blazor for Web Forms Developers ✨

Are you a Web Form developer? This book is brand new and will help you bridge your knowledge between WebForms and Blazor.

Everything is linked below (expect more updates in later months) but you may want to start at the beginning.

> ❗ Do not forget that you can download all eBooks with the **Download PDF** button in the bottom left of your screen (desktop) or through the `Content` menu if you are on mobile. ❗

* [Blazor for ASP.NET Web Forms Developers](https://docs.microsoft.com/dotnet/architecture/blazor-for-web-forms-developers/index?wt.mc_id=)
  - [App startup](https://docs.microsoft.com/dotnet/architecture/blazor-for-web-forms-developers/app-startup?wt.mc_id=)
  - [Architecture comparison of ASP.NET Web Forms and Blazor](https://docs.microsoft.com/dotnet/architecture/blazor-for-web-forms-developers/architecture-comparison?wt.mc_id=)
  - [Build reusable UI components with Blazor](https://docs.microsoft.com/dotnet/architecture/blazor-for-web-forms-developers/components?wt.mc_id=)
  - [App configuration](https://docs.microsoft.com/dotnet/architecture/blazor-for-web-forms-developers/config?wt.mc_id=)
  - [Data access and management](https://docs.microsoft.com/dotnet/architecture/blazor-for-web-forms-developers/data?wt.mc_id=)
  - [Forms and validation](https://docs.microsoft.com/dotnet/architecture/blazor-for-web-forms-developers/forms-validation?wt.mc_id=)
  - [Blazor app hosting models](https://docs.microsoft.com/dotnet/architecture/blazor-for-web-forms-developers/hosting-models?wt.mc_id=)
  - [An introduction to Blazor for ASP.NET Web Forms developers](https://docs.microsoft.com/dotnet/architecture/blazor-for-web-forms-developers/introduction?wt.mc_id=)
  - [Modules, handlers, and middleware](https://docs.microsoft.com/dotnet/architecture/blazor-for-web-forms-developers/middleware?wt.mc_id=)
  - [Migrate from ASP.NET Web Forms to Blazor](https://docs.microsoft.com/dotnet/architecture/blazor-for-web-forms-developers/migration?wt.mc_id=)
  - [Pages, routing, and layouts](https://docs.microsoft.com/dotnet/architecture/blazor-for-web-forms-developers/pages-routing-layouts?wt.mc_id=)
  - [Project structure for Blazor apps](https://docs.microsoft.com/dotnet/architecture/blazor-for-web-forms-developers/project-structure?wt.mc_id=)
  - ['Security: authentication and authorization in ASP.NET Web Forms and Blazor'](https://docs.microsoft.com/dotnet/architecture/blazor-for-web-forms-developers/security-authentication-authorization?wt.mc_id=)
  - [State management](https://docs.microsoft.com/dotnet/architecture/blazor-for-web-forms-developers/state-management?wt.mc_id=)

### eBook Update - Architecting Cloud Native .NET Applications for Azure

Are you a developer, a lead or even an architect? Want to know how to build application that are made for the cloud?

This is the book for you. Lots of new articles and updated chapters after the 3.0 GA release.

> ❗ Do not forget that you can download all eBooks with the **Download PDF** button in the bottom left of your screen (desktop) or through the `Content` menu if you are on mobile. ❗

* [Architecting Cloud Native .NET Applications for Azure](https://docs.microsoft.com/dotnet/architecture/cloud-native/?wt.mc_id=)
  - [Logging with Elastic Stack](https://docs.microsoft.com/dotnet/architecture/cloud-native/logging-with-elastic-stack?wt.mc_id=)
  - [Scaling containers and serverless applications](https://docs.microsoft.com/dotnet/architecture/cloud-native/scale-containers-serverless?wt.mc_id=)
  - [Service Mesh communication infrastructure](https://docs.microsoft.com/dotnet/architecture/cloud-native/service-mesh-communication-infrastructure?wt.mc_id=)
  - [Service-to-service communication](https://docs.microsoft.com/dotnet/architecture/cloud-native/service-to-service-communication?wt.mc_id=)
  - [Front-end client communication](https://docs.microsoft.com/dotnet/architecture/cloud-native/front-end-communication?wt.mc_id=)
  - [Leveraging containers and orchestrators](https://docs.microsoft.com/dotnet/architecture/cloud-native/leverage-containers-orchestrators?wt.mc_id=)
  - [Leveraging serverless functions](https://docs.microsoft.com/dotnet/architecture/cloud-native/leverage-serverless-functions?wt.mc_id=)

### eBook - ASP.NET Core gRPC for WCF Developers - gRPC for WCF Developers 💥✨

Lots of you have asked me last month about how to handle gRPC coming from a WCF point of view. Well, this month is your time to shine. There's a literal book being currently written about it. The whole thing is brand new is aimed at developers that were, surprise surprise, using WCF before.

This books take you from migration, load balancing, kubernetes, error handling, security, to why even use it. It is a must read.

> ❗ Do not forget that you can download all eBooks with the **Download PDF** button in the bottom left of your screen (desktop) or through the `Content` menu if you are on mobile. ❗

* [ASP.NET Core gRPC for WCF Developers - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/index?wt.mc_id=)
  - [Appendix - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/appendix?wt.mc_id=)
  - [Application Performance Management - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/application-performance-management?wt.mc_id=)
  - [How gRPC approaches RPC - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/approach?wt.mc_id=)
  - [Call credentials - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/call-credentials?wt.mc_id=)
  - [Channel credentials - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/channel-credentials?wt.mc_id=)
  - [Create gRPC client libraries - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/client-libraries?wt.mc_id=)
  - [Create a new ASP.NET Core gRPC project - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/create-project?wt.mc_id=)
  - [Docker - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/docker?wt.mc_id=)
  - [Encryption and network security - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/encryption?wt.mc_id=)
  - [Error handling - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/error-handling?wt.mc_id=)
  - [gRPC in production - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/grpc-in-production?wt.mc_id=)
  - [Overview of gRPC - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/grpc-overview?wt.mc_id=)
  - [Interface Definition Language - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/interface-definition-language?wt.mc_id=)
  - [Introduction - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/introduction?wt.mc_id=)
  - [Kubernetes - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/kubernetes?wt.mc_id=)
  - [Load balancing gRPC - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/load-balancing?wt.mc_id=)
  - [Metadata - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/metadata?wt.mc_id=)
  - [Migrate WCF duplex services to gRPC - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/migrate-duplex-services?wt.mc_id=)
  - [Migrate a WCF request-reply service to gRPC - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/migrate-request-reply?wt.mc_id=)
  - [Migrate a WCF solution to gRPC - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/migrate-wcf-to-grpc?wt.mc_id=)
  - [Network protocols - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/network-protocols?wt.mc_id=)
  - [Protobuf Any and Oneof fields for variant types - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/protobuf-any-oneof?wt.mc_id=)
  - [Protobuf scalar data types - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/protobuf-data-types?wt.mc_id=)
  - [Protobuf enumerations - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/protobuf-enums?wt.mc_id=)
  - [Protobuf maps for dictionaries - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/protobuf-maps?wt.mc_id=)
  - [Protobuf messages - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/protobuf-messages?wt.mc_id=)
  - [Protobuf nested types - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/protobuf-nested-types?wt.mc_id=)
  - [Repeated fields for lists and arrays - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/protobuf-repeated?wt.mc_id=)
  - [Protobuf reserved fields - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/protobuf-reserved?wt.mc_id=)
  - [Protocol buffers - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/protocol-buffers?wt.mc_id=)
  - [Types of RPC - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/rpc-types?wt.mc_id=)
  - [Security in gRPC applications - gRPC for WCF developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/security?wt.mc_id=)
  - [Self-hosted gRPC applications - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/self-hosted?wt.mc_id=)
  - [Service meshes - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/service-mesh?wt.mc_id=)
  - [gRPC streaming services versus repeated fields - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/streaming-versus-repeated?wt.mc_id=)
  - [WCF bindings and transports - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/wcf-bindings?wt.mc_id=)
  - [WCF endpoints and gRPC methods - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/wcf-endpoints-grpc-methods?wt.mc_id=)
  - [Comparing WCF to gRPC - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/wcf-services-to-grpc-comparison?wt.mc_id=)
  - [Why gRPC is recommended for WCF developers - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/why-grpc?wt.mc_id=)
  - [WS-* protocols - gRPC for WCF Developers](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/ws-protocols?wt.mc_id=)

### Compatibility 💥

If something broke, it's listed here. They are all important, if it impacts you. Go through the list and read those that applies to your current environment.

* [Breaking changes, version 2.2 to 3.0 - .NET Core](https://docs.microsoft.com/dotnet/core/compatibility/2.2-3.0?wt.mc_id=)
* [Breaking changes, version 3.0 Preview 6 to 3.0 Preview 7 - .NET Core](https://docs.microsoft.com/dotnet/core/compatibility/3.0.6-3.0.7?wt.mc_id=)
* [Breaking changes, version 3.0 Preview 7 to 3.0 Preview 8 - .NET Core](https://docs.microsoft.com/dotnet/core/compatibility/3.0.7-3.0.8?wt.mc_id=)
* [Breaking changes, version 3.0 Preview 8 to 3.0 Preview 9 - .NET Core](https://docs.microsoft.com/dotnet/core/compatibility/3.0.8-3.0.9?wt.mc_id=)
* [Breaking changes, version 3.0 Preview 9 to 3.0 - .NET Core](https://docs.microsoft.com/dotnet/core/compatibility/3.0.9-3.0?wt.mc_id=)
* [.NET Core breaking changes](https://docs.microsoft.com/dotnet/core/compatibility/breaking-changes?wt.mc_id=)
* [CoreFx breaking changes - .NET Core](https://docs.microsoft.com/dotnet/core/compatibility/corefx?wt.mc_id=)
* [Cryptography breaking changes, version 2.2 to 3.0 - .NET Core](https://docs.microsoft.com/dotnet/core/compatibility/crypto?wt.mc_id=)
* [Cryptography breaking changes - .NET Core](https://docs.microsoft.com/dotnet/core/compatibility/cryptography?wt.mc_id=)
* [Breaking changes, .NET Framework to .NET Core 3.0 - .NET Core](https://docs.microsoft.com/dotnet/core/compatibility/framework-core?wt.mc_id=)
* [Globalization breaking changes - .NET Core](https://docs.microsoft.com/dotnet/core/compatibility/globalization?wt.mc_id=)
* [Visual Basic breaking changes - .NET Core](https://docs.microsoft.com/dotnet/core/compatibility/visualbasic?wt.mc_id=)
* [Windows Forms breaking changes - .NET Core](https://docs.microsoft.com/dotnet/core/compatibility/winforms?wt.mc_id=)

### .NET Desktop Guide - What is Windows Presentation Foundation 💥✨

New guide about WPF for .NET Core, what it is and its place within .NET Core 3.0. Whether you like XAML or not, It's a good thing to remember as there are tons of WPF applications in the wild and your chances of encountering one is mostly guaranteed.

This guide may save you some time in the future.

* [What is Windows Presentation Foundation](https://docs.microsoft.com/dotnet/desktop-wpf/overview/index?wt.mc_id=)
  - [Data binding in WPF - .NET Desktop](https://docs.microsoft.com/dotnet/desktop-wpf/data/data-binding-overview?wt.mc_id=)
  - [Create a style in WPF - .NET Desktop](https://docs.microsoft.com/dotnet/desktop-wpf/fundamentals/styles-templates-create-apply-style?wt.mc_id=)
  - ['Styles & Templates in WPF - .NET Desktop'](https://docs.microsoft.com/dotnet/desktop-wpf/fundamentals/styles-templates-overview?wt.mc_id=)
  - [Define XAML resources for WPF - .NET Desktop](https://docs.microsoft.com/dotnet/desktop-wpf/fundamentals/xaml-resources-define?wt.mc_id=)
  - [XAML overview (WPF) - .NET Desktop](https://docs.microsoft.com/dotnet/desktop-wpf/fundamentals/xaml?wt.mc_id=)
  - [Migrating WPF Apps to .NET Core 3.0 - .NET Desktop](https://docs.microsoft.com/dotnet/desktop-wpf/migration/convert-project-from-net-framework?wt.mc_id=)
  - [Differences between .NET Framework WPF - .NET Desktop](https://docs.microsoft.com/dotnet/desktop-wpf/migration/differences-from-net-framework?wt.mc_id=)

### .NET Framework

Updates to the actual .NET Framework. Not-Core related.

* [How to: Build a multifile assembly](https://docs.microsoft.com/dotnet/framework/app-domains/build-multifile-assembly?wt.mc_id=)
* [How to: Build a .NET Framework single-file assembly](https://docs.microsoft.com/dotnet/framework/app-domains/build-single-file-assembly?wt.mc_id=)
* [How to: Share an assembly with other applications](https://docs.microsoft.com/dotnet/framework/app-domains/how-to-share-an-assembly-with-other-applications?wt.mc_id=)
* [How to: Get type and member information by using reflection](https://docs.microsoft.com/dotnet/framework/reflection-and-codedom/get-type-member-information?wt.mc_id=)
* [Storyboards Overview](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/storyboards-overview?wt.mc_id=)

### .NET Guide - Assemblies in .NET

Everything you ever wanted or didn't want to know about assemblies in .NET. To sign or not to sign isn't the question. Did you know? That is the question.

* [Assemblies in .NET](https://docs.microsoft.com/dotnet/standard/assembly/index?wt.mc_id=)
* [Assembly contents](https://docs.microsoft.com/dotnet/standard/assembly/contents?wt.mc_id=)
* [How to: Create a public-private key pair](https://docs.microsoft.com/dotnet/standard/assembly/create-public-private-key-pair?wt.mc_id=)
* [How to: Create signed friend assemblies](https://docs.microsoft.com/dotnet/standard/assembly/create-signed-friend?wt.mc_id=)
* [How to: Create unsigned friend assemblies](https://docs.microsoft.com/dotnet/standard/assembly/create-unsigned-friend?wt.mc_id=)
* [Delay-sign an assembly](https://docs.microsoft.com/dotnet/standard/assembly/delay-sign?wt.mc_id=)
* [Walkthrough: Embed types from managed assemblies in Visual Studio](https://docs.microsoft.com/dotnet/standard/assembly/embed-types-visual-studio?wt.mc_id=)
* [How to: Find an assembly's fully qualified name](https://docs.microsoft.com/dotnet/standard/assembly/find-fully-qualified-name?wt.mc_id=)
* [Friend assemblies](https://docs.microsoft.com/dotnet/standard/assembly/friend?wt.mc_id=)
* [How to: Determine if a file is an assembly](https://docs.microsoft.com/dotnet/standard/assembly/identify?wt.mc_id=)
* [How to: Load and unload assemblies](https://docs.microsoft.com/dotnet/standard/assembly/load-unload?wt.mc_id=)
* [How to: Reference a strong-named assembly](https://docs.microsoft.com/dotnet/standard/assembly/reference-strong-named?wt.mc_id=)
* [How to: Sign an assembly with a strong name](https://docs.microsoft.com/dotnet/standard/assembly/sign-strong-name?wt.mc_id=)

### Tooling, Tutorial, Serialization, Exceptions, and Others

Machine learning tutorial? Yes. We haven't gotten enough of them that's for sure. Machine learning is becoming omnipresent and starting with a tutorial sure is a good way to learn.

* ['Tutorial: Analyze sentiment of movie reviews using a pre-trained TensorFlow model'](https://docs.microsoft.com/dotnet/machine-learning/tutorials/text-classification-tf?wt.mc_id=)
* [Load data from files and other sources](https://docs.microsoft.com/dotnet/machine-learning/how-to-guides/load-data-ml-net?wt.mc_id=)

The JSON serializer is brand new. Have you tried it? Those articles are your perfect start to understanding how it works. Including the most fun data type to serialize, `DateTime` and `DateTimeOffset`.

* [How to serialize JSON - .NET](https://docs.microsoft.com/dotnet/standard/serialization/system-text-json-how-to?wt.mc_id=)
* [JSON serialization in .NET](https://docs.microsoft.com/dotnet/standard/serialization/system-text-json-overview?wt.mc_id=)
* [DateTime and DateTimeOffset support in System.Text.Json](https://docs.microsoft.com/dotnet/standard/datetime/system-text-json-support?wt.mc_id=)

With the new tooling and .NET being used on the daily, you're bound to run into issues. The first article handle the possible issues you might run into. The second, shows you how to make localized exception messages. Neat 🤖📷.

* ✨ [Troubleshoot .NET Core tool usage issues](https://docs.microsoft.com/dotnet/core/tools/troubleshoot-usage-issues?wt.mc_id=)
* ✨ [How to: create user-defined exceptions with localized exception messages](https://docs.microsoft.com/dotnet/standard/exceptions/how-to-create-localized-exception-messages?wt.mc_id=)

## .NET APIs

?????????

## ASP.NET Core

* 📢 [What's new in ASP.NET Core 3.0](https://docs.microsoft.com/aspnet/core/release-notes/aspnetcore-3.0?wt.mc_id=&view=aspnetcore-3.0)
* 💥 [Migrate from ASP.NET Core 2.2 to 3.0](https://docs.microsoft.com/aspnet/core/migration/22-to-30?wt.mc_id=&view=aspnetcore-3.0)

### Blazor

If you are now reading this, it's because you are a fan of WebAssembly(aka: WASM). Enjoy this small update.

Server-side Blazor has hit GA while client-side is still in Preview.

* [Create and use ASP.NET Core Razor components](https://docs.microsoft.com/aspnet/core/blazor/components?wt.mc_id=&view=aspnetcore-3.0)
* [ASP.NET Core Blazor hosting models](https://docs.microsoft.com/aspnet/core/blazor/hosting-models?wt.mc_id=&view=aspnetcore-3.0)

### Fundamentals

Lots of fundamental articles has been updated to the 3.0 release. 

You will find here updated articles, samples, and general tidying of articles (typos, more snippets, clarifications).

* [Detect changes with change tokens in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/change-tokens?wt.mc_id=&view=aspnetcore-3.0)
* [Dependency injection in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection?wt.mc_id=&view=aspnetcore-3.0)
* [Background tasks with hosted services in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services?wt.mc_id=&view=aspnetcore-3.0)
* [Microsoft.AspNetCore.App metapackage for ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?wt.mc_id=&view=aspnetcore-3.0)
* [Middleware activation with a third-party container in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/extensibility-third-party-container?wt.mc_id=&view=aspnetcore-3.0)
* [Factory-based middleware activation in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/extensibility?wt.mc_id=&view=aspnetcore-3.0)
* [ASP.NET Core Middleware](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/index?wt.mc_id=&view=aspnetcore-3.0)
* [Routing in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/routing?wt.mc_id=&view=aspnetcore-3.0)
* [Kestrel web server implementation in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel?wt.mc_id=&view=aspnetcore-3.0)

### gRPC

So you're not the *book* kind of developer. You'd rather jump in the code and try right away. I get that. After you get up to date as to what is gRPC on .NET Core, we're getting right into how to integrate with gRPC services. Straight to the code.

* [Introduction to gRPC on .NET Core](https://docs.microsoft.com/aspnet/core/grpc/index?wt.mc_id=&view=aspnetcore-3.0)
* ✨ [Call gRPC services with the .NET client](https://docs.microsoft.com/aspnet/core/grpc/client?wt.mc_id=&view=aspnetcore-3.0)
* ✨ [gRPC client factory integration in .NET Core](https://docs.microsoft.com/aspnet/core/grpc/clientfactory?wt.mc_id=&view=aspnetcore-3.0)
* ✨ [Logging and diagnostics in gRPC on .NET](https://docs.microsoft.com/aspnet/core/grpc/diagnostics?wt.mc_id=&view=aspnetcore-3.0)

### MVC

Updated article about testing controller logic. Finally, there were some docs missing about Tag Helpers. This is now a solved problem. See below.

* [Test controller logic in ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/controllers/testing?wt.mc_id=&view=aspnetcore-3.0)
* ✨ [Link Tag Helper in ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/link-tag-helper?wt.mc_id=&view=aspnetcore-3.0)
* ✨ [Script Tag Helper in ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/script-tag-helper?wt.mc_id=&view=aspnetcore-3.0)

### SignalR

We were missing a page about what feature each of our client supports. Check out the brand new page below. Then, since SignalR has dropped `UseSignalR()` everywhere and you now need to use `UseEndpoints(...)`, all of our docs now accurately represent how to set this up.

* ✨ [SignalR client features](https://docs.microsoft.com/aspnet/core/signalr/client-features?wt.mc_id=&view=aspnetcore-3.0)
* [Authentication and authorization in ASP.NET Core SignalR](https://docs.microsoft.com/aspnet/core/signalr/authn-and-authz?wt.mc_id=&view=aspnetcore-3.0)
* [Host ASP.NET Core SignalR in background services](https://docs.microsoft.com/aspnet/core/signalr/background-services?wt.mc_id=&view=aspnetcore-3.0)
* [ASP.NET Core SignalR configuration](https://docs.microsoft.com/aspnet/core/signalr/configuration?wt.mc_id=&view=aspnetcore-3.0)
* [Security considerations in ASP.NET Core SignalR](https://docs.microsoft.com/aspnet/core/signalr/security?wt.mc_id=&view=aspnetcore-3.0)
* [Use ASP.NET Core SignalR with TypeScript and Webpack](https://docs.microsoft.com/aspnet/core/tutorials/signalr-typescript-webpack?wt.mc_id=&view=aspnetcore-3.0)

### WebAPI

After releasing the `Microsoft.dotnet-openapi` global tool, it is important to have documentation for it. That's in the first article. A new article on customizing your error handling with ASP.NET Core Web API is definitely a must read as well.

For the rest, you will find here updated samples, and general tidying of articles (typos, more snippets, clarifications).

* ✨ [Develop ASP.NET Core apps using OpenAPI](https://docs.microsoft.com/aspnet/core/web-api/Microsoft.dotnet-openapi?wt.mc_id=&view=aspnetcore-3.0)
* ✨ [Handle errors in ASP.NET Core web APIs](https://docs.microsoft.com/aspnet/core/web-api/handle-errors?wt.mc_id=&view=aspnetcore-3.0)
* [Controller action return types in ASP.NET Core web API](https://docs.microsoft.com/aspnet/core/web-api/action-return-types?wt.mc_id=&view=aspnetcore-3.0)
* [Format response data in ASP.NET Core Web API](https://docs.microsoft.com/aspnet/core/web-api/advanced/formatting?wt.mc_id=&view=aspnetcore-3.0)
* [Create web APIs with ASP.NET Core](https://docs.microsoft.com/aspnet/core/web-api/index?wt.mc_id=&view=aspnetcore-3.0)

### Host and Deploy

Tons of updates on hosting with the new release of 3.0 in GA. With the release of Blazor Server-Side, this documentation also needed major updates. If you are using Blazor today, make sure to read this to avoid problems in the future. Health checks now get wrapped under `UseEndpoints` which required its documentation to be changed as well.

* [ASP.NET Core Module](https://docs.microsoft.com/aspnet/core/host-and-deploy/aspnet-core-module?wt.mc_id=&view=aspnetcore-3.0)
* 💥 [Host and deploy ASP.NET Core Blazor](https://docs.microsoft.com/aspnet/core/host-and-deploy/blazor/index?wt.mc_id=&view=aspnetcore-3.0)
* [Health checks in ASP.NET Core](https://docs.microsoft.com/aspnet/core/host-and-deploy/health-checks?wt.mc_id=&view=aspnetcore-3.0)

### Performance

Updated package names and new snippets.

* [Distributed caching in ASP.NET Core](https://docs.microsoft.com/aspnet/core/performance/caching/distributed?wt.mc_id=&view=aspnetcore-3.0)

### Razor Pages

Introduction has been completely rewritten with new code snippets for 3.0. Worth taking the time to refresh your knowledge on Razor Pages.

* 💥 [Introduction to Razor Pages in ASP.NET Core](https://docs.microsoft.com/aspnet/core/razor-pages/index?wt.mc_id=&view=aspnetcore-3.0)
* [Reusable Razor UI in class libraries with ASP.NET Core](https://docs.microsoft.com/aspnet/core/razor-pages/ui-class?wt.mc_id=&view=aspnetcore-3.0)

### Security

Updated for `UseEndpoints()` as well as including new API updates for 3.0. Updated documentation troubleshoot certification update. 

* [Use cookie authentication without ASP.NET Core Identity](https://docs.microsoft.com/aspnet/core/security/authentication/cookie?wt.mc_id=&view=aspnetcore-3.0)
* [Enable Cross-Origin Requests (CORS) in ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/cors?wt.mc_id=)
* [Enforce HTTPS in ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/enforcing-ssl?wt.mc_id=&view=aspnetcore-3.0)

### Tests

* [Integration tests in ASP.NET Core](https://docs.microsoft.com/aspnet/core/test/integration-tests?wt.mc_id=&view=aspnetcore-3.0)

## NuGet

* 📢 [NuGet 5.3 Release Notes](https://docs.microsoft.com/nuget/release-notes/NuGet-5.3?wt.mc_id=)
* 💥 [Deprecating packages on nuget.org](https://docs.microsoft.com/nuget/nuget-org/Deprecate-packages?wt.mc_id=)

### Reference

* [.nuspec File Reference for NuGet](https://docs.microsoft.com/nuget/reference/nuspec?wt.mc_id=)
  - deprecating `iconUrl`, adding `icon` to replace it.
* [NuGet pack and restore as MSBuild targets](https://docs.microsoft.com/nuget/reference/msbuild-targets?wt.mc_id=)
  - adding instructipon on how to pack an `icon` within your package
