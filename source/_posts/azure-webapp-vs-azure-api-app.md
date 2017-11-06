---
title : "Azure WebApp vs Azure API App"
date: 2016-10-11 12:00:00
tags: [azure]
---

> **tldr;**  No differences under the hood. Only different icons, name and an API Definition that is populated. All App Services features are still available to you.

If it's your first time in the Azure ecosystem, you must be wondering what is the difference between a WebApp and an API app. Which one should I choose?

### Differences between Azure WebApp and Azure API App

Most of the differences are pretty much in the naming, the icons, and the tooling.

Features of one are available in the others. There is absolutely no differences beside icons and names on the Azure Portal. Your initial choice doesn't impact you as much as it once did.

Let's take a sample web app (created as an Azure WebApp) that is in my Azure Subscription. Here's what is displayed once I get in the menu.

![Mobile and Api menu](/posts/files/difference-between-azure-webapp/mobile-api.png)

Both Api and Mobile options are available. Where's the other differences? Mainly in the tooling. Some elements of the tooling is only going to work if you have an API definition (see above) but having an API Definition is not exclusive to an API App.

The API Definition is a link to your Swagger 2.0 API description. When publishing an API Service from Visual Studio, this field is going to be set automatically but can be pretty much anything you want.

Once you have an API defined, multiple other scenarios opens up like exposing your APIs through logic apps or BizTalk services, using API Management, or generating an API client from Visual Studio. But at its core? It's still an App Service.

#### Generating a Client from a WebApp API Definition

When I right click `Add > REST Api Client...` in a Console Application in Visual Studio 2015, I'm shown the following screen.

![Add REST Client](/posts/files/difference-between-azure-webapp/client.png)

Clicking `Select Azure Asset...` will bring you this window.

![Select Azure Asset](/posts/files/difference-between-azure-webapp/azure-asset.png)

As you can see, there's no API present. What happens if I publish a web app and add the API Definition after?

![Set API Definition](/posts/files/difference-between-azure-webapp/set-api-definition.png)

I'll close the Azure Asset Selector and refresh it.

![Select Azure Asset](/posts/files/difference-between-azure-webapp/azure-with-assets.png)

### Summary

There once was a disconnect between Azure WebApp and API App. Today? The only difference is which icon/name you want that app to be flagged with. Otherwise? All features available for one is available to the other.

So go ahead. Create an app. Whether it's an API, a Website, or an hybrid doesn't matter. You'll get work done and deployed just as easily. 
