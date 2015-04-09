---
title: "Code Snippet - Quickly changing the host in an URL in C#"
date: 2009-02-03 11:48:00
tags: [snippet, c#]
---

Quick code snippet today. Let's say you have some URLs that you want to change the hostname/port on it without having to do string manipulation.

```cs
//multiple constructor are available
UriBuilder builder = new UriBuilder("http://localhost:1712")
    {
        Host = Environment.MachineName,
        Port = 80
    };
Uri myNewUri = builder.Uri;
```

There you go! It's as easy as this. This will avoid countless hour of parsing a URL for the hostname, port number or whatever else you are searching.

The bonus with that is that the path of the URL won't be touched at all.

Have fun!