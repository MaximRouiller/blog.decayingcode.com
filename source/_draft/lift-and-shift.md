---
title : "The Lift and Shift Cloud Migration Strategy"
date: 2016-10-17 08:00
tags: [azure]
---

When migrating assets to the cloud, there is many way to do it. Nothing is a silver bullet. Each scenario need to be adapted to your particular situation.

Among the many ways to do things here are a few that are commonly known.

We'll start with the Lift and Shift.

### Lift and shift

> **TLDR;** You want the benefit of the cloud, but can't afford the downtime of building for the cloud.

The lift and shift cloud migration strategy is to take your local assets (mostly on VMs) and just move them to the cloud without major change in code or configurations.

### Why lift and shift?

You want to do a lift and shift when your application architecture is just too complex to be Cloud Native or that you don't have the time to convert it. Maybe you have a 3rd party software that needs to be installed on a VM or that the application assumes that it controls the whole machine and requires elevation. In those scenario, where re-architecting big parts of the application is just going to be too expensive (or too time consuming), a lift and shift approach is a good migration strategy.

### Caveats

This isn't the path if you want to save huge amount of money quickly. You will save on cost but not by as huge a margin if the application was built for the cloud.  You might need to modify your application a bit to ensure that you ensure that you just run on the cloud but the changes will be mostly minor.

You will probably still need to manage your Virtual Machine and have them follow your IT practices that you had before. Microsoft will not manage those for you. You are 100% in control of your compute.

### Gains

It is however one way to reach scalability that wasn't previously available. You are literally removing the risk with handling your own hardware and putting it on your cloud provider. Let's be honest here, they have way more money in the infrastructure game to ensure that whatever you want to run, it will. They have layers upon layers of redundancy from power to physical security of those servers. Data centers have certifications that would cost you millions to get. With Azure, you can get this. Now.

So stop focusing on hardware and redundancy. Start focusing on your business and your company's goal. Unless you are competing in the same sphere as Microsoft and Amazon, your business isn't hosting. So let's focus on what matters. Focus on your customers and how to bring them value.

With some change to the initial architecture, you will also be able to scale out (multiple VMs) your application server based on your VM's usage. Or with no change to the architecture, you can increase (or lower) the power of the VM with just a few click from the Portal or a script. This can definitely bring you and edge where a competitor would have to build a new server to allow for expanding their infrastructure.

You? You get those on demand for how long or how short you need them.

Those gains are the low hanging fruits and will be available to pretty much all other cloud migration strategy.

### When to re-architecture

#### Big Data

The cloud offers many things that allows you to focus on the business rather than on the technical details. If you have a farm of servers internally doing Big Data, you might want to avoid converting those to the cloud. Those might just end up costing you more money than you would save.

What could be done is leverage Azure Big Data offering. Azure offers [HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/), [Machine Learning](https://azure.microsoft.com/en-us/services/machine-learning/) and [SQL Data Warehouses](https://azure.microsoft.com/en-us/services/sql-data-warehouse/).

Most of those tools will allow you to sift through terabytes of data and allow you extract trends and valuable insights on your data. They will also be less costly than to run than a fleet of powerful virtual machines that are dedicated to this purpose.

#### Big Compute

If you have tons of computational job to do, you might think that having a local infrastructure is the only way to go. The problem you are facing is probably that you are capping your processing power or that you just can't transfer fast enough to those processing nodes. At that moment, you think about optimizing your servers, your network infrastructure, etc.

This is a race which has already been ran by Microsoft and other companies. You already have the data and how to process it. Let Microsoft handle the infrastructure. You don't need to build the perfect HPC farm. Check Microsoft [Big Compute](https://azure.microsoft.com/en-us/solutions/big-compute/) solution and do focus on solving problems rather than infrastructure.

#### Simpler applications

Simpler applications are perfect candidate for a refactoring. Most of them use a SQL Database and expose some HTML/JavaScript to the end user. Converting those out of a VM with IIS and moving them to Azure Websites and Azure SQL Database will end up lowering your cost my lots.

Multiple websites can share compute when they are barely used or just have a dedicated compute when they are mission critical.

Those are a few of the many scenarios that would be a good candidate for a move toward a new architecture.

### What's next

There's many way to evolve once your old legacy servers are in a VM on Azure.

Going with container is one of the many solutions. Of course, if you already have containers internally, your lift and shift to Azure should be about as painless as can be. If you are not, it will make your life easier by identifying the dependencies your old applications were using. This would allow you to leverage the cloud reliability and scalability without changing too much.

For certain application going Cloud Native is definitely going to save you more money as you adapt your application to only use the resources it needs. By using Blob Storage instead of the file system, NoSQL storage where it makes sense and activating auto scaling, you will be able to save your customer money and run a more lean business.

Do you agree? Is there scenarios that I missed? Please comment bellow!
