---
title: "Easy deployment with IIS and Web Deploy with Visual Studio"
date: 2013-05-09 07:00:00
tags: [asp.net,deployment,iis]
---

You’ve probably all seen the Publish Method called “Web Deploy” in your Visual Studio 2010/2012 publish popup. On my end, I’ve used the classic “push to a local folder then copy/paste” method of deploying.

Today, I’m going to show you how to install it on your IIS and make it work with Visual Studio

### Installing Web Deploy

The first thing to do is [downloading the IIS extension](http://www.iis.net/downloads/microsoft/web-deploy) and executing it. It should open the Web Platform Installer and suggest you the right version of Web Deploy and then click **Install**.

Then we need to go to IIS to confirm that it is properly installed. Right clicking on a Site should bring you this menu:

[![webdeploy_installed](/posts/files/webdeploy_installed_thumb.png "webdeploy_installed")](/posts/files/webdeploy_installed.png)

### Deploying through Visual Studio
  > **IMPORTANT**
> 
> You need to run Visual Studio as an Administrator for this to work.  

Now when you try to publish an application through Visual Studio, select “Web Deploy” from the drop down and type in your URL.

**Visual Studio 2010:**

[![publishing_through_webdeploy](/posts/files/publishing_through_webdeploy_thumb.png "publishing_through_webdeploy")](/posts/files/publishing_through_webdeploy.png)

**Visual Studio 2012:**

[![publish_vs2012](/posts/files/publish_vs2012_thumb.png "publish_vs2012")](/posts/files/publish_vs2012.png)

In Visual Studio, you could always use the option called **Build Deployment Package** so that if you have to give a package to your sysadmin (or a client) to deploy, it can be done with single Zip and an package to import. Included in this package can be, folder authorization, registry keys that are required and more.

### Why should I use Web Deploy?

*   You are not required to be an administrator on the server to publish an application.*   Allow for partial differential deployment (only what changed)
*   You can push the database with Entity Framework and run migrations on the different databases*   Synchronize web servers between themselves (IIS6/IIS7/IIS8)*   Easily give a package for a client to deploy
*   Easily deploy from a Build server (Continuous integration)
*   [Automatically backup before publishing](http://www.iis.net/learn/publish/using-web-deploy/web-deploy-automatic-backups)  

### Conclusion

Web Deploy is already in V3 and barely been talked about in our spheres. Maybe we are too development oriented and are not looking at making our tasks easier. In any case, installing the web deploy extension on an IIS server should only take you a few minutes and will be worth it very fast by making your deployments easier than ever.

For more information, do not miss the [MSDeploy blog](http://blogs.iis.net/msdeploy/). Or the initial [blog post](http://weblogs.asp.net/scottgu/archive/2010/09/13/automating-deployment-with-microsoft-web-deploy.aspx) by Scott Guthrie… over two and a half years ago.