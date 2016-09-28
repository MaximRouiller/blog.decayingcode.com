---
title : "Creating .NET Core Console WebJobs on an ASP.NET Core Azure WebApp"
date: 2016-09-28 12:00
tags: [azure, webjobs,.net core, asp.net core]
---

> **Code tested on .NET Core RTM 1.0.1 with Tooling Preview2 and Azure SDK 2.9.5**

### Introduction

I love WebJobs. You deploy a website, you need a few tasks to be run on the side to ensure that everything is running smoothly or maybe un-queue some messages and do some actions.

How to do it in ASP.NET 4.6.2? Easy. Visual Studio is throwing you menus, wizards, blog posts and what not at you to help you start using it. The tooling is curated to make you fall into a pit of success.

With .NET Core tooling still being in preview, let's just say that there's no menus nor wizards to help you out. I haven't seen anything online that help you automate the "*Publish*" experience.

### The basics - How do they even work?

WebJobs are easy. If you forget the Visual Studio tooling for a moment, WebJobs are run by [Kudu][1].

Kudu is the engine behind most of Azure WebApps experience from deployments to WebJobs. So how does Kudu detect that you have WebJobs to run? Well, [everything you need to know](https://github.com/projectkudu/kudu/wiki/Web-Jobs) is on their wiki.

Basically, you will have a command file named `run` (with various supported extensions) located somewhere like this:
`app_data/jobs/{Triggered|Continous}/{jobName}/run.{cmd|ps1|fsx|py|php|sh|js}`

If that file is present? Bam. WebJob created with the `jobName`. It will automatically show in your Azure Portal.

Let's take this `run.cmd` for example:

```bash
@echo off

echo "Hello Azure!"
```

### Setting a schedule

The simplest way is to create a CRON schedule. This can be done by creating a `settings.job` file. This file is basically all the WebJobs options. One of them is called schedule.

If you want to trigger a job every hour, just copy/paste the following in your `settings.job` file.


```json
{
	"schedule": "0 0 */1 * * ? *"
}
```

### What we should have right now

0. One ASP.NET Core application project ready to publish
    * A `run.cmd` file and a `settings.job` file under `/app_data/jobs/Triggered/MyWebJob`
0. One .NET Core Console Application

To make sure it runs, we need to ensure that we publish the `app_data` folder. So add/merge the following section to your `project.json`:

```json
{
  "publishOptions": {
    "include": [
      "app_data/jobs/**/*.*"      
    ]
  },
}
```

If you deploy this right now, your web job will run every hour and echo `Hello Azure!`.

Let's make it useful.

### Publishing a Console Application as a WebJobs

First let's change `run.cmd`.
```bash
@echo off

MyWebJob.exe
```

Or if your application doesn't specify a `runtimes` section in your `project.json`:

```bash
@echo off
dotnet MyWebJob.dll
```

The last part is where the fun begins. Since we are publishing the WebApp, those executable do not exist. We need to ensure that they are created.

There's a section in your `project.json` that runs right after publishing the ASP.NET Core application.

Let's publish our WebJob directly into the proper `app_data` folder.

```json
{
  "scripts": {
    "postpublish": [
      "dotnet publish ..\\MyWebJob\\ -o %publish:OutputPath%\\app_data\\jobs\\Triggered\\MyWebJob\\"
    ]
  }
}
```

### Final Result

Congratulation! You now have a .NET Core WebJob published in your ASP.NET Core application on Azure that runs every hour.

All those console application are of course runnable directly locally or through the Azure scheduled trigger.

If this post helped, let me know in the comments!

[1]: https://github.com/projectkudu/kudu
