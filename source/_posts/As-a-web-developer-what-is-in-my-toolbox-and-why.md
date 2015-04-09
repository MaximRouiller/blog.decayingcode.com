---
title: "As a web developer, what is in my toolbox and why"
date: 2014-03-06 13:00:00
tags: [tools]
---

A few of my fellow developers in Montreal are not web developers and often asks me how to debug certain problem on the web and what tools I am using.

Today, I’m sharing all of my tools and why I’m using them. So let’s start, shall we?

## Web-based tools

### [Json2Csharp.com](http://json2csharp.com/)

I’m using this little converter when I’m parsing external REST service and that I want to create simple C# DTO out of the exposed object. They start as DTO but gets enriched rather quickly.

It takes a string as this:
```json
{
   "employees":[
      {
         "firstName":"John",
         "lastName":"Doe"
      },
      {
         "firstName":"Anna",
         "lastName":"Smith"
      },
      {
         "firstName":"Peter",
         "lastName":"Jones"
      }
   ]
}
```

And transforms it into this:

```cs
public class Employee
{
    public string firstName { get; set; }
    public string lastName { get; set; }
}

public class RootObject
{
    public List&lt;Employee&gt; employees { get; set; }
}
```

That makes it easy for JSON converter tools to map JSON to strongly-typed entities.

### [JsonFormatter (CuriousConcept.com)](http://jsonformatter.curiousconcept.com/)

Talking about JSON, this tool transforms any JSON string into different format (3 spaced, 2 spaced or compact). It will also validate the JSON at the same time.

Taking the above JSON that I used in the previous example, it can transform it into this:
```json
{"employees":[{"firstName":"John","lastName":"Doe"},{"firstName":"Anna","lastName":"Smith"},{"firstName":"Peter","lastName":"Jones"}]}
```

So if you service returns it to you into a very compact format, you can easily expand it to visualize the data.

## Desktop Tools (All Windows based, sorry Mac/Linux people)

### Visual Studio 2013

As I’m a heavy .NET developer… I still use Visual Studio for most of what I do. Considering that the JavaScript and CSS editor have improved a lot… it has become a tool of importance especially for deployment on the cloud which it makes easy as pie.

### [JetBrains ReSharper](http://www.jetbrains.com/resharper/)

Not quite there yet for JavaScript but awesome on the C# side of thing.

### [Fiddler by Telerik](http://www.telerik.com/fiddler)

This is a free tool. It basically serves as a proxy on your local machine capturing all web traffics over the wire. It allows you to filter and create web requests. Very excellent when you start debugging some specific HTTP call that you make and understand exactly what is happening on the wire.

### Google Chrome

Simply because its debugging tools for JavaScript are beyond compare. You can debug JavaScript, forge request for different user agents, set device ratio and sizes, edit live CSS and this without breaking a sweat. 

If you are into heavy CSS/JavaScript modification live on the server, Chrome supports you to set a “Workspace” and the website you are on will sync (both ways) modification made to either side of things. Kind of crazy if you are into Single Page Applications.

## References

### [CanIUse.com](http://caniuse.com)

You are building an application and you are wondering if you can use [HTML5 videos](http://caniuse.com/#feat=video) or [Local Storage](http://caniuse.com/#feat=namevalue-storage) with IE8? This website will cover you front to back for most cases. If you are wondering which specs are implemented in a browser related to HTML5, CSS or SVG… this is the site you want to go on to before using them.

## What are you using?

Any tools that I left out? Share them with everyone else! I would be curious if there is a way to do more with what we already have!