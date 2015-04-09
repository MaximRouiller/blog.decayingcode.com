---
title: "How to parse an RSS and ATOM feed from pure .NET without plugins"
date: 2014-02-18 09:30:00
tags: [asp.net]
---

Let’s throw an hypothetic scenario… you want to know when one of your favorite blogger post something. Maybe that blogger post his updates to Twitter and you can follow that. But sometimes they don’t. Or maybe you are building a system to aggregate their content.

Then you start looking for plugins on how to do it properly and you don’t find much.

Let me show you how to do it without plugins.

## Prerequisite

*   .NET 3.5 SP1 

&nbsp;

## Step 1 : Retrieve the data

First let’s make our code compatible with multiple feeds. We’ll collect a list of URLs from a few very popular blogs.
```cs
var rssFeeds = new List<Uri>
{
    new Uri("http://weblogs.asp.net/scottgu/rss.aspx"),
    new Uri("http://feeds.hanselman.com/ScottHanselman"),
    new Uri("http://blogs.msdn.com/b/dotnet/rss.aspx"),

};
var client = new HttpClient();

foreach (var rssFeed in rssFeeds)
{
    var result = client.GetStreamAsync(rssFeed).Result;

    // todo: implement the rest
}
```

## Step 2: Parse the data to retrieve the proper value

Okay, now we have the RAW XML from any RSS or Atom feed. We’ll need to parse that data somehow. Let me introduce you to the SyndicationFeed.

```cs
using (var xmlReader = XmlReader.Create(result))
{
    SyndicationFeed feed = SyndicationFeed.Load(xmlReader);

    if (feed != null)
    {
        // todo: loop over feed.Items?
    }
}
```

The SyndicationFeed is a built-in RSS and Atom parser found in the .NET Framework. No need for plugins here. 

In my scenario however, I needed the Author name. With those 3 feed, you will find none of them if you are inspecting the code. Here’s what is fun however. The format is extensible. 

Let’s retrieve an extension value.

## Step 3: Retrieve Extension Values like “dc:creator” or “dc:publisher”

So those extensions are described in the [Dublin Core](http://dublincore.org/documents/2012/06/14/dcmi-terms/?v=elements#). As an example, you can see [creator](http://dublincore.org/documents/2012/06/14/dcmi-terms/?v=elements#elements-creator) and [publisher](http://dublincore.org/documents/2012/06/14/dcmi-terms/?v=elements#elements-publisher). Those are used in pretty much all Blog Engines like WordPress and BlogEngine.NET. 

You will find those extension on each different blog post which is represented in C# by the SyndicationItem (in the array feed.Items from earlier). Here are the extensions method I built to retrieve them:

```cs
public static class SyndicationItemExtensions
{
    public static string GetCreator(this SyndicationItem item)
    {
        var creator = item.GetElementExtensionValueByOuterName("creator");
        return creator;
    }

    public static string GetPublisher(this SyndicationItem item)
    {
        var publisher = item.GetElementExtensionValueByOuterName("publisher");
        return publisher;
    }

    private static string GetElementExtensionValueByOuterName(this SyndicationItem item, string outerName)
    {
        if (item.ElementExtensions.All(x => x.OuterName != outerName)) return null;
        return item.ElementExtensions.Single(x => x.OuterName == outerName).GetObject<XElement>().Value;
    }
}
```

Now we should be able to retrieve the author quite easily.

## Step 4: Access the values

So let’s rewrite Step 2 but with us trying to find the author of an RSS feed.

```cs
using (var xmlReader = XmlReader.Create(result))
{
    SyndicationFeed feed = SyndicationFeed.Load(xmlReader);

    if (feed != null)
    {
        var item = feed.Items.First();

        string creator = item.GetCreator();
        string publisher = item.GetPublisher();

        var blogAuthor = (feed.Authors.FirstOrDefault() ?? new SyndicationPerson()).Name;
        var chosenAuthor = creator ?? publisher ?? blogAuthor;
        Console.WriteLine("   Chosen author: {0}", chosenAuthor);
    }
}
```

Here is the output from Console
```text
   Chosen author: ScottGu
   Chosen author: Scott Hanselman
   Chosen author: The .NET Fundamentals Team
```

## Conclusion

So without any extensions, I was able to parse RSS feeds. If you have a blog, you should try it on your own and see how that works. On my end, my blog was misconfigured by showing “My Name” as one of those entries.

So if you need more info, don’t hesitate to reach me on Twitter [@MaximRouiller](https://twitter.com/MaximRouiller). 

Enjoy!