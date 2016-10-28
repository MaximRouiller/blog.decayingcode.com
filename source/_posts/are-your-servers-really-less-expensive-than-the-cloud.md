---
title : "Are your servers really less expensive than the cloud?"
date: 2016-10-28 08:00
tags: [azure]
---

I've had clients in the past that were less than convinced by the cloud.

Too expensive. Less control and too hard to use.

Control [has already been addressed by Troy Hunt](https://www.troyhunt.com/with-great-azure-vm-comes-great/) before and I feel that it translate very well to this article.

As for the difficulty to use the cloud, it's seriously just a [few clicks away in Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-deploy/) or a [GitHub synchronization](https://github.com/blog/2056-automating-code-deployment-with-github-and-azure).

That leaves us with pricing which can be tricky. Please note that the prices were taken at the time of writing and will change in the future.

### Too expensive

#### On-Premise option
When calculating the Total Cost of Ownership (TCO) of a server, you have to include all direct, indirect and invisible costs that goes into running this server.

First, there's the server itself. If I go VERY basic with one machine, I can get one for $1300 or $42/months over 36 months with financing. We're talking bare metal server here. No space or RAM for virtualization. I could probably get something cheap at Best Buy but at that point, it's consumer-level quality no warranty on business usage. If you are running a web server and a database, you want those to be on two distinct machine.

Then, unless you have the technical knowledge and time to setup a network, configure the machine, secure them you will have to pay someone to pay for this. We're talking either $100/hours of work or hiring someone who can do the job. Let's assume you have a consultant come over for 2 days' worth to help you set that up (16 hours). Let's add 5 hours per years to deal with updates, upgrades and other.

Once that server is configured and ready to run, I hope you have AC to keep it cool. Otherwise, your 36 months investment may have components break sooner rather than later. If you don't have it "on-site" but rather rent a space in a colocation area, keep that amount in mind for the next part.

Now let's do a run down. $1300 per server, 16 hours at $100 per hours (with 5 hours per years to keep the machine up and running), add the electrical cost of running the server, the AC or a colocation rent.

Over 36 months, we're at $158 per months + electrical / rent. I haven't included cost of backups or any other redundancies.

#### Cloud Option

Let's try to get you something similar on the cloud.

First thing you will need is a database. On Azure, a standard S1 database will run you at $0.403/hours or about $30 per months. Most applications these days require a database so that goes without saying.

Now you need to host your application somewhere. If you want to handle everything from network to Windows Updates, a Standard A1 virtual machine will run you at $0.090/hours or $67 per months.

But let's be real here. If you are developing an API or a Web Application, let's drop having to manage everything. Let's check AppServices. You can get a Basic B1 instance for $0.075/ hours or $56 per months.

What does AppServices brings you however is no Windows Updates. Nothing to maintain. Just make sure your app is running and alive. That's your only responsibility.

With Azure, running with Virtual Machines will cost you a bit less than $100 per months and AppServices at around $86 per months.

Let's say I add 100Gb of storage and 100Gb of bandwidth usage per months and here's what I get.

![Estimate for AppServices](/posts/files/azure-cost/app-services.estimate.png)

#### Fine prints

If you analyze in details what I've thrown at you in terms of pricing, you might see that an *On-Premise* server is way more powerful than a Cloud VM. That is true. But in my experience, nobody is using 100% of their resources anyway. If you do not agree with me, let's meet in the comments!

Here's what's not included in this pricing. You can be hosted anywhere in the world. You can start your hosting in United States but find out that you have clients in Europe? You can either duplicate your environment or move it to this new region without problems. In fact, you could have one environment per region without huge efforts on your end. Depending on the plans you picked, you can load balance or auto-scale what was deployed on the cloud. Impossible to do with On-Premise without waiting for new machines. Since you can have more than one instances per region and multiple deployment over many regions, your availability becomes way higher than anything you could ever achieve alone. 99.95% isn't impossible. We're talking less than 4.5 hours of downtime per year or less than 45 seconds every day. With a physical server, one busted hard-drive will take you down for at least a day.

### Take-away notes

That you agree or not with me, one thing to take away is to always include all costs.

Electricity, renting space, server replacement, maintenance, salary, consulting costs, etc. All must be included to fairly compare all options.

### Summary

Some things should stay local to your business. From your desktop computers to your router, those need to stay local. But with tools like Office 365, which takes your email/files to the cloud, and Azure, which takes your hosting to the cloud. Less and less elements require a proper local server farm anymore.

Your business or your client's business can literally run off of somebody else's hardware.

What do you think? Do you agree with me? Do you disagree? Let me know in the comments.
