---
title : "The Lift and Shift Cloud Migration Strategy"
date: 2016-10-17 08:00
tags: [azure]
---

When migrating assets to the cloud, there is many way to do it.

There is Lift and Shift, The Cloud Native and the Refactored way.

### Lift and shift

> **TLDR;** You want the benefit of the cloud, but can't afford the downtime of building for the cloud.

The lift and shift cloud migration strategy is to take your local assets (mostly on VMs) and just move them to the cloud without major change in code or configurations.

### Why lift and shift?

You want to do a lift and shift when your application architecture is just too complex to be Cloud Native or that you don't have the time to convert it. Maybe you have a 3rd party software that needs to be installed on a VM or that the application assumes that it controls the whole machine and requires elevation. In those scenario, where re-architecting big parts of the application is just going to be too expensive (or too time consuming), a lift and shift approach is a good migration strategy.

### Caveats

This isn't the path if you want to save huge amount of money quickly. You will save on cost but not by as huge a margin if the application was built for the cloud.  You might need to modify your application a bit to ensure that you ensure that you just run on the cloud but the changes will be mostly minor.

You will probably still need to manage your Virtual Machine and have them follow your IT practices that you had before. Microsoft will not manage those for you. You are 100% in control of your compute.

### Gains

It is however one way to reach scalability that wasn't previously available. With some change to the initial architecture, you will be able to Scale Out (multiple VMs) your application server based on metrics and performance counters. Or with no change to the architecture, you can increase (or lower) the power of the VM with just a few click from the Portal or a script.

Those gains are the low hanging fruits and will be available to pretty much all other cloud migration strategy.

### When to re-architecture

#### Big Data
The cloud offers many things that allows you to focus on the business rather than on the technical details. If you have a farm of servers internally doing Big Data, you might want to avoid converting those to the cloud. Those might just end up costing you more money than you would save.

What could be done is leverage Azure Big Data offering. Azure offers [HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/), [Machine Learning](https://azure.microsoft.com/en-us/services/machine-learning/) and [SQL Data Warehouses](https://azure.microsoft.com/en-us/services/sql-data-warehouse/).

Most of those tools will allow you to sift through terabytes of data and allow you extract trends and valuable insights on your data. They will also be less costly than to run than a fleet of powerful virtual machines that are dedicated to this purpose.

#### Simpler applications

Simpler applications are perfect candidate for a refactoring. Most of them use a SQL Database and expose some HTML/JavaScript to the end user. Converting those out of a VM with IIS and moving them to Azure Websites and Azure SQL Database will end up lowering your cost my lots.

Multiple websites can share compute when they are barely used or just have a dedicated compute when they are mission critical.

Those are a few of the many scenarios that would be a good candidate for a move toward a new architecture.


### What's next

Going Cloud Native is definitely going to save you more money as you adapt your application to only use the resources it needs. But why go native? We'll see that in the next article.

Do you agree? Is there scenarios that I missed? Please comment bellow!
