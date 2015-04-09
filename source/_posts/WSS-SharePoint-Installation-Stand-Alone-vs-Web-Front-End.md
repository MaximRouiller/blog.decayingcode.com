---
title: "WSS/SharePoint Installation - Stand-Alone vs Web Front-End"
date: 2009-01-19 15:05:00
tags: []
---

Small post about that today. I ended up losing a good part of my day reinstalling a SharePoint installation on a VMWare machine. Why?

Because at first I installed it in Stand-Alone mode which sounded like a good idea at first. What does that do is that it creates a SQL Server Express database and won't allow anyone connections on it. Adding insult to injury, it also doesn't work with all the normal components of a SharePoint installation.

So... after reverting back to a working snapshot... redoing all my Windows Update plus reinstalling SharePoint... I end up with a working installation.

So... as a small reminder... don't forget to specify "Web Front-End" when first prompted by the installation. It will save you a lot of time

