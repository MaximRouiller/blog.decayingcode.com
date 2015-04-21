---
title: "Problem deploying a solution package on a SharePoint 2007 farm?"
date: 2009-02-13 23:19:00
tags: []
---

You have a WSP and you are trying to deploy it inside the farm and it doesn't work. You hate it. You try to look in the Event Viewer, SharePoint logs, etc.

Before you event start looking around everywhere, be sure to know that when you are adding/deploying a solution package you need some specify right.

Here is a small checklist:

*   DBO access to the Configuration database (normally end with "_Config")
*   Farm administrator of the SharePoint site

But you are probably why it was working on the development machine and not in production?

##### Why am I not DBO?

Normally when a development machine is created, the developers are Administrators on the machine. Most SharePoint/WSS installations are made on a SQL Server 2005 installation. When installing SharePoint on a SQL Server 2005, all administrators of the machine where the database is installed are DBO. However, in a production environment, developers are given more restricted access and often doesn't have DBO access on the database.

##### **Why am I not Farm administrator?**

As for the Farm administrator situation, the user that configure SharePoint on the development machine is automatically given Farm Administrators right which is not the case in Production environment.

##### Conclusion

It is really important to know which minimum permissions are required to do certain task inside SharePoint 2007\. This is a specific case where only trusted users are allowed to make such system wide changes. SharePoint 2007 is configured to be "secured by default" and restricted to disallow any unauthorized user to make changes that could compromise the farm.

Enjoy your SharePoints deployments
