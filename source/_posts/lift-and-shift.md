---
title : "The Lift and Shift Cloud Migration Strategy"
date: 2016-10-21 08:00:00
tags: [azure]
---

When migrating assets to the cloud, there are many ways to do it. Nothing is a silver bullet. Each scenario needs to be adapted to our particular situations.

Among the many ways to migrate current assets to a cloud provider, one of the most common is called Lift and Shift.

### Lift and shift

> **TLDR;** You want the benefit of the cloud, but can't afford the downtime of building for the cloud.

The lift and shift cloud migration strategy are to take your local assets (mostly on VMs or pure VMs) and just move them to the cloud without major changes in your code.

### Why lift and shift?

Compared to the alternatives of building for the cloud, called Cloud Native, or refactor huge swath of your application to better use the cloud, it's easy to see why. It just saves a lot of developer hours.

### When to lift and shift?

You want to do a lift and shift when your application architecture is just too complex to be Cloud Native or that you don't have the time to convert it. Maybe you have a 3rd party software that needs to be installed on a VM or that the application assumes that it controls the whole machine and requires elevation. In those scenarios, where re-architecting big parts of the application are just going to be too expensive (or too time consuming), a lift and shift approach is a good migration strategy.

### Caveats

Normally, when going to the cloud, you expect to save lots of money. At least, that's what everybody promises.

That's one of the issues with a lift and shift. This isn't the ideal path to save a huge amount of money quickly. Since you will be billed in terms of resources used/reserved, and if your Virtual Machines are sitting there doing nothing, it would be a good idea to start consolidating a few of them. But besides the cost, the move should be the most simple of all migration strategies.

You'll still need to manage your Virtual Machine and have them follow your IT practices and patching schedule. Microsoft will not manage those for you. You are 100% in control of your compute.

### Gains

It is, however, one way to reach scalability that wasn't previously available. You are literally removing the risk with handling your own hardware and putting it on your cloud provider. Let's be honest here, they have way more money in the infrastructure game to ensure that whatever you want to run, it will. They have layers upon layers of redundancy from power to physical security. Data centers have certifications that would cost you millions to get. With Azure, you can get this. Now.

So stop focusing on hardware and redundancy. Start focusing on your business and your company's goal. Unless you are competing in the same sphere as Microsoft and Amazon, your business isn't hosting. So let's focus on what matters. Focus on your customers and how to bring them value.

With some change to the initial architecture, you will also be able to scale out (multiple VMs) your application servers based on a VM's metric. Or with no changes to the architecture, you can increase (or lower) the power of the VM with just a few click from the Portal or a script. It's the perfect opportunity to save money by downgrading a barely used Application Server or shutting down unused machines during off-hours.

This can definitely bring you and edge where a competitor would have to build a new server to allow for expanding their infrastructure. You? You get those on demand for how long or how short you need them.

The capability to scale and the reliability of the cloud are the low hanging fruits and will be available to pretty much all other cloud migration strategy and with all cloud providers.

### When to re-architecture

#### Massive databases

We used to put everything in a database. I've seen databases in gigabytes or even terabytes territory. That's huge.

Unless those databases are Data Warehouses, you want to keep your database as slim as possible to save money and gain performance.

If you are storing images and files in an SQL database, it might be time to use Azure Blob Storage. If you have a Data Warehouse, don't store it in SQL Azure but rather store it in Azure SQL Data Warehouse.


#### Big Compute

If you are processing an ungodly amount of images, videos or mathematical models on a VM, it's time to start thinking High-Performance Computing (HPC).

Of course, lift and shifting your VM to the cloud is a nice temporary solution but most likely, you aren't using those VMs 100% of the time they are up. When they are up, they make take longer to run the task than you might like.

It's time to check Azure Batch, Cognitive Services (if doing face recognition) or even Media Services if you are doing video encoding.

Those services allow you to scale to ungodly level to match your amount of work. Rather than keeping your VMs dedicated to those workloads, taking the time to refactor the work to be done so that they can better leverage Azure Services will allow you to improve your processing time and reduce the amount of maintenance on those machines.

#### Small Web Applications

Do you have small web applications with one small database hooked on an IIS Server on your network? Those are an excellent candidate to be moved to an Azure WebApp.

If your application has low traffic, low CPU/memory usage and is sitting with many other similar apps on a VM, they are the perfect scenario for a refactoring to Azure App Services.

Those applications can be put on a Basic App Service Plan and share computing resources. If one of those apps is suddenly more resources intensive, it takes 10 minutes to give it its own computing resources.

Even the database themselves are the perfect case to move to an Azure SQL Elastic Pool where database shares their compute.

With the proper tweaking, it's possible to gain all the benefits of simple website hosting without having to handle the VMs and their maintenance.

### What's next

There's many ways to evolve once your old legacy servers are in a VM on Azure.

Going with containers is one of the many solutions. Of course, if you already have containers internally, your lift and shift to Azure should be about as painless as can be. If you are not, it will make your life easier by identifying the dependencies your old applications were using. This would allow you to leverage the cloud reliability and scalability without changing too much.

Without containers, consider [Azure Site Recovery](https://azure.microsoft.com/en-us/services/site-recovery/) to automate replication of VMs on the cloud.

For certain application going Cloud Native is definitely going to save you more money as you adapt your application to only use the resources it needs. By using Blob Storage instead of the file system, NoSQL storage where it makes sense and activating auto scaling, you will be able to save money and run a leaner business.

Do you agree? Are there scenarios that I missed? Please comment bellow!
