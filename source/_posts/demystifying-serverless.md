---
title : "Demystifying serverless"
date: 2017-07-28 10:45:00
tags: [azure, serverless]
---

I've been working on building and deploying applications for over 15 years. I remember clearly trying to deploy my first application on a Windows Server 2000. The process was long. Tedious. First, you had to have the machine. Bigger companies could afford a data center and a team to configure it. Smaller companies? It was that dusty server you often find beside the receptionist when you walk-in the office. Then, you had to figure out permissions, rolling logs, and all those other things you need to do to deploy a web server successfully.

And your code isn't even deployed yet. You only have the machine.

Imagine having to deal with that today. It would be totally unacceptable. Yet, we still do it.

# Present

If you're lucky, your company have moved that server to a colocated data center or even as VMs to the cloud.

If you already made the move to Platform as a Service, everything is ok. Right?

Definitely not! We're still stuck creating machines. Even though it doesn't look like it, we're still managing dusty servers. We're still doing capacity planning, auto-scaling, selecting plans to keep our costs under control. So many things that you need to plan before you put down a single line of code.

There must be a better way.

Imagine an environment where you just deploy your code and the cloud figure out what you need.

# Serverless

Let me talk about serverless. One of those buzzword bingo terms that comes around often. Let's pick it apart and make sure we truly understand what it means.

Serverless isn't about not having servers. Serverless is not even about servers.

It's about letting you focus on your code. On your business.

What makes you different from your competitors?

What will make **you** successful?

Provisioning server farms? Capacity planning? This doesn't make you different. It just makes you slow.

Serverless is about empowering you to build what differentiates you from the other business down the road.

Don't focus on infrastructure.

Focus on what makes you amazing.

# What is Azure Functions?

Azure Functions is Microsoft's implementation of serverless. It allows you to run code in a multitude of languages, from any environment and deploy it in seconds.

Creating an Azure Function from [the CLI](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function-azure-cli?WT.mc_id=maximerouiller-blog-marouill), the [Azure Portal](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function?WT.mc_id=maximerouiller-blog-marouill) or even [Visual Studio](https://docs.microsoft.com/azure/azure-functions/functions-create-your-first-function-visual-studio?WT.mc_id=maximerouiller-blog-marouill) is just as simple as it should be.

Azure Functions will only charge you for what you use. If your API isn't used, it's free. As your business grows and your application starts getting customers, it's when you start paying. You're not spending money until you start making money.

The best part? You get enough free executions per months to start your next startup, a micro-SaaS or the glue to integrate existing systems.

Stop postponing it. [Try Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-serverless-api?WT.mc_id=maximerouiller-blog-marouill). Right now.

We're here.

We want you to succeed.

Let us help you be amazing.
