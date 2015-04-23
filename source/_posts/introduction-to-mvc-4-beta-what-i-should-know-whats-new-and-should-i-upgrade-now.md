---
title: "Introduction to MVC 4 Beta – What I should know, what’s new and should I upgrade now?"
date: 2012-03-02 19:36:08
tags: []
---

# What’s new?

A ton of stuff. We are talking here about the WebAPI, async support with the new .NET 4.5 async keywords, async HttpModules and HttpHandlers, Bundling and minifications, Single Page Applications (referred to as SPA), mobile project templates and the integrated AzureSDK.

Wow. That’s a pretty big release isn’t it? Let’s go through all those items one by one and give you all a small explanation of what it is.

## WebAPI

WebAPI is an attempt by Microsoft to make the HTTP protocol a first class citizen. It looks like WCF (basic HTTP binding) but it’s not. In fact, it’s not using any of the WCF assemblies. Then, it uses the routes of MVC… but it’s not a content rendering engine.

WebAPI is a new service API to render service data based on the HTTP request. As an example, if you do send the HTTP header “Accept: text/json”, it will return JSON. No more mentioning that in the URL anymore. It’s the closest we have ever been to a RESTful service. Bonus point, it doesn’t use the JSON encoding of WCF (which was awfully slow) but the one from NewtonSoft (JSON.NET).

## Bundling and Minification

Remember the previous post I did on how to bundle and minify your JS and CSS files for MVC3? Well now it’s integrated. You don’t need to do that anymore. It’s part of the default template. No more custom package from me. You compile and it works.

The advantage of this technique is that you can now upgrade your jQuery library without having to change its reference in the _Layout.cshtml or your masterpage. It will be automatically upgraded. The team as already received the feedback that the invocation call to the “ResolveBundleUrl” was a tad too long so no point in complaining.

## Single Page Application

This is the first draft to making web pages in a single page. They are using knockout.js for the client-side data-binding and rendering, upshot.js for the JavaScript data access layer, WebAPI on the server side and jQuery for the goo that holds the world together. This deserves a whole post by itself but it’s a very promising technology. 

## Mobile Project Templates

A new project template based on jQuery Mobile. This will make your web pages look like a native mobile application. The UI is touch optimized, and easy to get the hang with.

## AzureSDK

All ASP.NET projects now comes with the ASP.NET Universal Providers and will by the same time allow you to upload any web applications to the Azure platform without much modification to your application. This will reduce the friction between how we develop applications for the cloud and how we develop applications for a single server.

## 

# Should I upgrade now?

Of course not. This is all but a beta and most of you won’t need any of those new technologies immediately. However, the core MVC remains the same and you could easily upgrade to the best and latest and enjoy the new features.

Please be remembered that it’s only a beta and nothing in the API is promised to stay exactly the same. However, if you are ready to take a controlled risk, Microsoft has given it a “Go-live” license and you are just a few minutes away from running the best and latest from MVC.

# Wait… you forgot the async stuff

Yeah… It’s pretty much the same as before but with the new keywords. I would new a whole new post to display those.
