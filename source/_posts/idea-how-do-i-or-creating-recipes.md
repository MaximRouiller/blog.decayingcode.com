---
title: "Idea: How do I...? Or creating recipes."
date: 2016-06-27 12:00
tags: [community, tool, azure]
---

> NOTE: This is a draft idea. My rambling is included.

My mind's been stuck on an idea recently.

We have some very good tools in our coding community to help us find solutions to problems. Tools like StackOverflow are simply too amazing to just move aside.

What's been missing, I think, are cookbooks. Or rather, list of recipes that includes snippets, solutions, explanation and community driven details.

### Attempt at mocking

![Sample mockup](/posts/files/howdoi.png)

### Recipe Model

Recipes should be centered around a few technologies and could be tagged for more precise scenarios. Let's say I want to integrate Azure Application Insights to Angular JS.

What is popping in my head is:

> How do I integrate application insights into AngularJS?

The model of said recipes could look like this:

```plain
Name: Integrate Application Insights into Angular JS
Tags: application insights, logging, exception
Technology: azure, angularjs, javascript
Description: [...] (markdown? html? bbformat?)
Snippets: [markdown? other?]
```

### Directions

One thing I definitely want is to make sure that this is hosted on Azure. Most recipes will never change so it's the perfect moment to go static. Having the description and the snippets in markdown would allow to quickly edit, and validate recipes.

As for the recipes themselves, I find that every client has their own recipes. Just like your mom's spaghetti, yours is always better than anyone else's but you do change it a bit overtime when encountering other recipes.

If this software is ever created, it should allow us to create a recipe book per user and allow us to share those recipes with others.

### Ramblings

I would love to see some AngularJS/ReactJS cookbooks. Maybe even some Microsoft Azure cookbooks to share snippets on how to work with Storage, Service Bus, etc.

Integrating in Visual Studio would be awesome. Especially with snippets that are just pure `web.config` or `*.json` modifications.

### Note

This is just an idea. If you run with it, meh. Go ahead. Ideas are worth nothing. Execution is the measure of success.
