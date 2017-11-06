---
title: "Taking your software on a static ride"
date: 2016-06-21 08:00:00
tags: [azure,static content,git]
---

If you haven't seen the change recently, I've changed, yet again, my blogging engine. However, this isn't a post about blogging or engines.

It's a post about static content.

### What was there before

Previously, I was on the [MiniBlog][1] blog engine written by [Mads Kristensen][2].

Despite a few issues I've found with it, the main argument for it is **Simple, flexible and powerful**. It's written on the GitHub repository after all.

### What is in place now

I currently run something called [Hexo][3]. It's a NodeJS static blog generator with support for themes and other modules if you are interested in customizing it.

### Why go static?

There is an inherent simplicity in having pure `.html` files as your content and simple folders as your *routing engine*. Over that, most web servers can handle local files very efficiently. Over that, they can output the proper caching HTTP headers without the need to reach other files or a database.

As for flexible, well it's HTML files. If I want images or any other files, I just push include them in the hierarchy of the blog.

You only need to configure your host a bit if you want to benefit from cached files.

### Caching

The `web.config` for this site contains the following:

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="365:00:00" />    
        </staticContent>
    </system.webServer>
</configuration>
```

You can find similar instructions for [nginx][4] or other servers/host pretty much everywhere.

### Azure & GitHub

The first step is to create an Azure Web App. If you have a custom domain, you will need to bump your plan to at least Shared. But you are just testing it out, it works with the Free plan.

* Once created, go into your Web application and click on settings.
* Navigate to Deployment source
* Choose source
* Pick GitHub and setup your authorization and pick a project/branch

This will synchronize your GitHub branch with your website. Every time there's a change on your branch, a new deployment will start.

The last piece of the puzzle is to actually generate your site with your static site generator. In my scenario, I needed to run `hexo generate`. When going through this process, you can inject a custom script into your repository to run custom commands while deploying. Mine can be found [right here][5].

### Result?

The final result is that I have a pure HTML/JS/CSS site that is running on a shared hosting.

As for all I know, it could run off of a [Raspberry Pi][6] or an [Arduino][7] plugged to the internet and nobody would know the difference. If my website ever becomes too popular for its own good, it will take a lot of visitors to slow it down. It is, after all, only serving static files.

### What changed in the architecture?

So what is a blog engine doing? It's reading post metadata (content, publishing date, authors, etc.) and converting them to HTML pages that the end-user is going to see. This blog engine is running off a web framework (ASP.NET MVC, Django, etc.) and this framework is running off a core language/runtime or something else. Finally, it is handing the content to the web server/host and this one is transferring it to the end-user.

What we've done is pre-generate all the possible results that the blog engine could ever produce and store them in static files (HTML). We've converted Dynamic to Static.

We had to drop a few functionality to get to this point like comments, live editing, pretty management UI... in my case, this is worth it for me. I'm technologically savvy enough to understand how to create a new post in my system without slowing me down. Like every architectural decision, it's all a matter of tradeoff.

### When should I use static content?

When data doesn't need to change too often. Most CMS, blogs and even e-commerce product description fit this bill. This could also be when you don't want to expose your database to external servers and instead just push content files.

You should also use static content if you need to host files on a multitude of different server OSes.

### But what to do with less tech-savvy users?

The same process can still be reused. Maybe not with a GitHub/Azure workflow.

However, you could always use the Azure Blob Storage and rewrite, using UrlRewrite, your URLs to the blob storage directly.

```xml
<rewrite>
    <rules>
        <rule name="imagestoazure">
            <match url="images/(.*)" />
            <action type="Redirect" url="https://??????.vo.msecnd.net/images/{R:1}" />
        </rule>
    </rules>
</rewrite>
```

That way, you can offer a rich UI to your user and re-generate your content when it change and upload it asynchronously to the blob storage. Your original website on Azure? No need to even take it down. The content change and the website keeps on forwarding requests.

### Conclusion

Yes, there is more moving pieces. But my belief is that it's only a tooling problem we have now. We have tools that were built to work with dynamic web sites. We're only starting to build them for static content.

Worse, we are thinking in MVC, cshtml and databases instead of just thinking about what the end client truly want. A web page with some content that hasn't changed in a long time.


[1]: https://github.com/madskristensen/MiniBlog
[2]: http://madskristensen.net/
[3]: https://hexo.io/
[4]: https://serversforhackers.com/nginx-caching
[5]: https://github.com/MaximRouiller/blog.decayingcode.com/blob/master/deploy.cmd
[6]: https://www.raspberrypi.org/documentation/remote-access/web-server/nginx.md
[7]: https://startingelectronics.org/software/arduino/web-server/basic-01/
