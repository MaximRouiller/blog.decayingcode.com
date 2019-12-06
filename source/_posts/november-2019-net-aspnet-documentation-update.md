---
title : "November 2019 .NET/ASP.NET Documentation Update"
date: 2019-12-06 10:00:00
tags: [documentation]
---

**TLDR; This is a status update on the .NET documentation. If you want me to do more of those (once a month), please let me know in the comments!**

**Comment: If you have suggestions, please let me know in the comments. Any product feedback will be forwarded to the proper product team.**

Hi everyone!

So the .NET Core 3.1 has finally released in the last few days, so expect lots of things related to this.

This update covers everything that happened since November 1st through December 4th. Although not everything has yet been updated for 3.1, expect a lot more coming next month!

If you still don't know me, my name is Maxime Rouiller and I'm a Cloud Advocate with Microsoft. For this month, I'm covering three major products:

* [.NET](https://github.com/dotnet/docs)
* [ASP.NET](https://github.com/aspnet/AspNetCore.Docs)

You'll find a nice legend below that explains and highlights articles that I think deserves special attention.

## Legend

* 📢: Major/Main article that everyone will want to read
* 💥: Important/Must read.
* ✨: Brand new page

Note: It's not because a page doesn't have an icon that it isn't important. Everything here is either brand new or significantly modified.

## Themes this month

* .NET
  - System.Text.Json documentation
  - C# 8.0 Updates
  - C# 8.0 Spec updates
  - New C# landing page
  - New API docs
  - SameSiteMode updates
* ASP.NET
  - React to identified problems with the 3.0 release
  - Article updates for 3.0
  - SameSiteMode updates

## .NET

* ✨ [FIPS compliance](https://docs.microsoft.com/dotnet/standard/security/fips-compliance?wt.mc_id=docsexp6_personal-blog-marouill)

### JSON 

* [How to serialize and deserialize JSON using C#](https://docs.microsoft.com/dotnet/standard/serialization/system-text-json-how-to?wt.mc_id=docsexp6_personal-blog-marouill)
* ✨ [How to write custom converters for JSON serialization](https://docs.microsoft.com/dotnet/standard/serialization/system-text-json-converters-how-to?wt.mc_id=docsexp6_personal-blog-marouill)

### API docs

We've added documentation for 444 APIs.

SameSiteMode updates:

* 💥 [System.Web.SameSiteMode](https://docs.microsoft.com/dotnet/api/system.web.samesitemode?wt.mc_id=docsexp6_personal-blog-marouill)
* 💥 [System.Web.HttpCookie.SameSite](https://docs.microsoft.com/dotnet/api/system.web.httpcookie.samesite?wt.mc_id=docsexp6_personal-blog-marouill)

## .NET Core

### Compatibility

* ✨ [Breaking changes, version 2.2 to 3.1](https://docs.microsoft.com/dotnet/core/compatibility/2.2-3.1?wt.mc_id=docsexp6_personal-blog-marouill)
* ✨ [Breaking changes, version 3.0 to 3.1](https://docs.microsoft.com/dotnet/core/compatibility/3.0-3.1?wt.mc_id=docsexp6_personal-blog-marouill)

### Installation

The old prerequisites articles were removed and in their place the following articles were created:

* ✨ [.NET Core SDK and runtime dependencies](https://docs.microsoft.com/dotnet/core/install/dependencies?wt.mc_id=docsexp6_personal-blog-marouill)
* ✨ [Check installed .NET Core versions on Windows, Linux, and macOS](https://docs.microsoft.com/dotnet/core/install/how-to-detect-installed-versions?wt.mc_id=docsexp6_personal-blog-marouill)
* ✨ [Install .NET Core - Linux](https://docs.microsoft.com/dotnet/core/install/linux-package-managers?wt.mc_id=docsexp6_personal-blog-marouill)
  - ✨ [Install .NET Core on Debian 10 - package manager](https://docs.microsoft.com/dotnet/core/install/linux-package-manager-debian10?wt.mc_id=docsexp6_personal-blog-marouill)
  - ✨ [Install .NET Core on Debian 9 - package manager](https://docs.microsoft.com/dotnet/core/install/linux-package-manager-debian9?wt.mc_id=docsexp6_personal-blog-marouill)
  - ✨ [Install .NET Core on CentOS 7 - package manager](https://docs.microsoft.com/dotnet/core/install/linux-package-manager-centos7?wt.mc_id=docsexp6_personal-blog-marouill)
  - ✨ [Install .NET Core on Fedora 29 - package manager](https://docs.microsoft.com/dotnet/core/install/linux-package-manager-fedora29?wt.mc_id=docsexp6_personal-blog-marouill)
  - ✨ [Install .NET Core on Fedora 30 - package manager](https://docs.microsoft.com/dotnet/core/install/linux-package-manager-fedora30?wt.mc_id=docsexp6_personal-blog-marouill)
  - ✨ [Install .NET Core on openSUSE 15 - package manager](https://docs.microsoft.com/dotnet/core/install/linux-package-manager-opensuse15?wt.mc_id=docsexp6_personal-blog-marouill)
  - ✨ [Install .NET Core on Linux RHEL 7 package manager](https://docs.microsoft.com/dotnet/core/install/linux-package-manager-rhel7?wt.mc_id=docsexp6_personal-blog-marouill)
  - ✨ [Install .NET Core on Linux RHEL 8.1 package manager](https://docs.microsoft.com/dotnet/core/install/linux-package-manager-rhel81?wt.mc_id=docsexp6_personal-blog-marouill)
  - ✨ [Install .NET Core on SLES 12 - package manager](https://docs.microsoft.com/dotnet/core/install/linux-package-manager-sles12?wt.mc_id=docsexp6_personal-blog-marouill)
  - ✨ [Install .NET Core on SLES 15 - package manager](https://docs.microsoft.com/dotnet/core/install/linux-package-manager-sles15?wt.mc_id=docsexp6_personal-blog-marouill)
  - ✨ [Install .NET Core on Ubuntu 16.04 package manager](https://docs.microsoft.com/dotnet/core/install/linux-package-manager-ubuntu-1604?wt.mc_id=docsexp6_personal-blog-marouill)
  - ✨ [Install .NET Core on Ubuntu 18.04 package manager](https://docs.microsoft.com/dotnet/core/install/linux-package-manager-ubuntu-1804?wt.mc_id=docsexp6_personal-blog-marouill)
  - ✨ [Install .NET Core on Ubuntu 19.04 package manager](https://docs.microsoft.com/dotnet/core/install/linux-package-manager-ubuntu-1904?wt.mc_id=docsexp6_personal-blog-marouill)
* ✨ [Install .NET Core on Windows, Linux, and macOS](https://docs.microsoft.com/dotnet/core/install/index?wt.mc_id=docsexp6_personal-blog-marouill)
* ✨ [Install .NET Core runtime on Windows, Linux, and macOS](https://docs.microsoft.com/dotnet/core/install/runtime?wt.mc_id=docsexp6_personal-blog-marouill)
* ✨ [Install .NET Core SDK on Windows, Linux, and macOS](https://docs.microsoft.com/dotnet/core/install/sdk?wt.mc_id=docsexp6_personal-blog-marouill)

### Runtime Configuration

* ✨ [Compilation config settings](https://docs.microsoft.com/dotnet/core/run-time-config/compilation?wt.mc_id=docsexp6_personal-blog-marouill)
* ✨ [Debugging profiling config settings](https://docs.microsoft.com/dotnet/core/run-time-config/debugging-profiling?wt.mc_id=docsexp6_personal-blog-marouill)
* ✨ [Garbage collector config settings](https://docs.microsoft.com/dotnet/core/run-time-config/garbage-collector?wt.mc_id=docsexp6_personal-blog-marouill)
* ✨ [Globalization config settings](https://docs.microsoft.com/dotnet/core/run-time-config/globalization?wt.mc_id=docsexp6_personal-blog-marouill)
* ✨ [Run-time config](https://docs.microsoft.com/dotnet/core/run-time-config/index?wt.mc_id=docsexp6_personal-blog-marouill)
* ✨ [Networking config settings](https://docs.microsoft.com/dotnet/core/run-time-config/networking?wt.mc_id=docsexp6_personal-blog-marouill)
* ✨ [Threading config settings](https://docs.microsoft.com/dotnet/core/run-time-config/threading?wt.mc_id=docsexp6_personal-blog-marouill)

### Testing

* [Unit testing C# code in .NET Core using dotnet test and xUnit](https://docs.microsoft.com/dotnet/core/testing/unit-testing-with-dotnet-test?wt.mc_id=docsexp6_personal-blog-marouill)

### Tooling

* ✨ [Get started with .NET Core using the CLI](https://docs.microsoft.com/dotnet/core/tutorials/using-with-xplat-cli?wt.mc_id=docsexp6_personal-blog-marouill)
* [Additions to the csproj format for .NET Core](https://docs.microsoft.com/dotnet/core/tools/csproj?wt.mc_id=docsexp6_personal-blog-marouill)
* [dotnet run command](https://docs.microsoft.com/dotnet/core/tools/dotnet-run?wt.mc_id=docsexp6_personal-blog-marouill)

## C\#

* [C# documentation](https://docs.microsoft.com/dotnet/csharp/index?wt.mc_id=docsexp6_personal-blog-marouill)
* ✨ [Nullable value types - C# reference](https://docs.microsoft.com/dotnet/csharp/language-reference/builtin-types/nullable-value-types?wt.mc_id=docsexp6_personal-blog-marouill)

## Desktop guide - WPF

* ✨ [Create a template in WPF](https://docs.microsoft.com/dotnet/desktop-wpf/themes/how-to-create-apply-template?wt.mc_id=docsexp6_personal-blog-marouill)
* [Styles and templates in WPF](https://docs.microsoft.com/dotnet/desktop-wpf/fundamentals/styles-templates-overview?wt.mc_id=docsexp6_personal-blog-marouill)

## F\#

* ✨ [What's new in F# - F# Guide](https://docs.microsoft.com/dotnet/fsharp/whats-new/index?wt.mc_id=docsexp6_personal-blog-marouill)
  - ✨ [What's new in F# 4.5 - F# Guide](https://docs.microsoft.com/dotnet/fsharp/whats-new/fsharp-45?wt.mc_id=docsexp6_personal-blog-marouill)
  - ✨ [What's new in F# 4.6 - F# Guide](https://docs.microsoft.com/dotnet/fsharp/whats-new/fsharp-46?wt.mc_id=docsexp6_personal-blog-marouill)
  - ✨ [What's new in F# 4.7 - F# Guide](https://docs.microsoft.com/dotnet/fsharp/whats-new/fsharp-47?wt.mc_id=docsexp6_personal-blog-marouill)
* [Sequences](https://docs.microsoft.com/dotnet/fsharp/language-reference/sequences?wt.mc_id=docsexp6_personal-blog-marouill)
* [Computation Expressions](https://docs.microsoft.com/dotnet/fsharp/language-reference/computation-expressions?wt.mc_id=docsexp6_personal-blog-marouill)
* [F# code formatting guidelines](https://docs.microsoft.com/dotnet/fsharp/style-guide/formatting?wt.mc_id=docsexp6_personal-blog-marouill)

## .NET for Apache Spark guide

* ✨ [Submit a .NET for Apache Spark job to Databricks](https://docs.microsoft.com/dotnet/spark/how-to-guides/databricks-deploy-methods?wt.mc_id=docsexp6_personal-blog-marouill)
* ✨ [Submit a .NET for Apache Spark job to Azure HDInsight](https://docs.microsoft.com/dotnet/spark/how-to-guides/hdinsight-deploy-methods?wt.mc_id=docsexp6_personal-blog-marouill)
* ✨ [Structured Streaming with .NET for Apache Spark tutorial](https://docs.microsoft.com/dotnet/spark/tutorials/streaming?wt.mc_id=docsexp6_personal-blog-marouill)
* [Get started with .NET for Apache Spark](https://docs.microsoft.com/dotnet/spark/tutorials/get-started?wt.mc_id=docsexp6_personal-blog-marouill)

## ML.NET

* ['Tutorial: Automated visual inspection using transfer learning'](https://docs.microsoft.com/dotnet/machine-learning/tutorials/image-classification-api-transfer-learning?wt.mc_id=docsexp6_personal-blog-marouill)

## ASP.NET Core

* ✨ [What's new in ASP.NET Core 3.1](https://docs.microsoft.com/aspnet/core/release-notes/aspnetcore-3.1?wt.mc_id=docsexp6_personal-blog-marouill)
* ✨ [Migrate from ASP.NET Core 3.0 to 3.1](https://docs.microsoft.com/aspnet/core/migration/30-to-31?wt.mc_id=docsexp6_personal-blog-marouill)

### Blazor

* ✨ [ASP.NET Core Blazor lifecycle](https://docs.microsoft.com/aspnet/core/blazor/lifecycle?wt.mc_id=docsexp6_personal-blog-marouill)
* ✨ [ASP.NET Core Blazor templates](https://docs.microsoft.com/aspnet/core/blazor/templates?wt.mc_id=docsexp6_personal-blog-marouill)
* [Create and use ASP.NET Core Razor components](https://docs.microsoft.com/aspnet/core/blazor/components?wt.mc_id=docsexp6_personal-blog-marouill)
* [ASP.NET Core Blazor forms and validation](https://docs.microsoft.com/aspnet/core/blazor/forms-validation?wt.mc_id=docsexp6_personal-blog-marouill)
* [Handle errors in ASP.NET Core Blazor apps](https://docs.microsoft.com/aspnet/core/blazor/handle-errors?wt.mc_id=docsexp6_personal-blog-marouill)
* [ASP.NET Core Blazor hosting models](https://docs.microsoft.com/aspnet/core/blazor/hosting-models?wt.mc_id=docsexp6_personal-blog-marouill)
* [Configure the Linker for ASP.NET Core Blazor](https://docs.microsoft.com/aspnet/core/host-and-deploy/blazor/configure-linker?wt.mc_id=docsexp6_personal-blog-marouill)
* [Host and deploy ASP.NET Core Blazor Server](https://docs.microsoft.com/aspnet/core/host-and-deploy/blazor/server?wt.mc_id=docsexp6_personal-blog-marouill)

### Performance

* ✨ [Memory management and patterns in ASP.NET Core](https://docs.microsoft.com/aspnet/core/performance/memory?wt.mc_id=docsexp6_personal-blog-marouill)

### Security

* ✨ [Overview of ASP.NET Core Authentication](https://docs.microsoft.com/aspnet/core/security/authentication/index?wt.mc_id=docsexp6_personal-blog-marouill)
* 💥✨ [Work with SameSite cookies in ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/samesite?wt.mc_id=docsexp6_personal-blog-marouill)
* [Add, download, and delete user data to Identity in an ASP.NET Core project](https://docs.microsoft.com/aspnet/core/security/authentication/add-user-data?wt.mc_id=docsexp6_personal-blog-marouill)
* [Configure certificate authentication in ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/authentication/certauth?wt.mc_id=docsexp6_personal-blog-marouill)
* [Policy-based authorization in ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/authorization/policies?wt.mc_id=docsexp6_personal-blog-marouill)
* [Role-based authorization in ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/authorization/roles?wt.mc_id=docsexp6_personal-blog-marouill)
* [Azure Key Vault Configuration Provider in ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration?wt.mc_id=docsexp6_personal-blog-marouill)

### Fundamentals

* [Use multiple environments in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/environments?wt.mc_id=docsexp6_personal-blog-marouill)
* [Make HTTP requests using IHttpClientFactory in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/http-requests?wt.mc_id=docsexp6_personal-blog-marouill)
* [Logging in .NET Core and ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index?wt.mc_id=docsexp6_personal-blog-marouill)

### Host and Deploy

* [Health checks in ASP.NET Core](https://docs.microsoft.com/aspnet/core/host-and-deploy/health-checks?wt.mc_id=docsexp6_personal-blog-marouill)

### MVC

* [Application Parts in ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/advanced/app-parts?wt.mc_id=docsexp6_personal-blog-marouill)
* [Model Binding in ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/models/model-binding?wt.mc_id=docsexp6_personal-blog-marouill)
* [Model validation in ASP.NET Core MVC](https://docs.microsoft.com/aspnet/core/mvc/models/validation?wt.mc_id=docsexp6_personal-blog-marouill)

### SignalR

* [Redis backplane for ASP.NET Core SignalR scale-out](https://docs.microsoft.com/aspnet/core/signalr/redis-backplane?wt.mc_id=docsexp6_personal-blog-marouill)
* [Differences between SignalR and ASP.NET Core SignalR](https://docs.microsoft.com/aspnet/core/signalr/version-differences?wt.mc_id=docsexp6_personal-blog-marouill)
