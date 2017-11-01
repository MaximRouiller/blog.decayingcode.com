---
title : "Breaking the shackles of server frameworks with static content"
date: 2017-11-01 11:00:34
tags: [static content, open source,wyam,hugo]
---

Let me tell you the story of my blog. 

It started back in 2009. Like everyone back then, I started on Blogger as it was the easiest way to start a blog and get it hosted for free. Pretty soon however, I wanted to have my own domain and start owning my content. 

This brings us down the rabbit hole of blog engines. Most engines back then were SQL based (SQL Server or MySQL) thus increasing the cost. 

My first blog engine was [BlogEngine.NET](https://github.com/rxtur/BlogEngine.NET). After a few painful upgrade, I decided to go to something easier and simpler. So I went with [MiniBlog](https://github.com/madskristensen/MiniBlog) by Mads Kristensen. Nothing fancy but I still had some issues that left me wanting to change yet again.

So this led me to thinking. What were the features in those blog engine that I used? I sure wasn't using the multi-user support. I'm blogging alone. As for the comments, I was using Disqus for years now due to easier spam management. What I was using was pushing HTML and having it published.

That's a very short list of feature. I asked the question... "Why am I using .NET?". Don't get me wrong, I love the .NET (and the new Core). But did I **NEED** .NET for this? I was basically just serving HTML/JS/CSS.

If I removed the .NET component what would happen?

## Advantages of no server components

First, I don't have to deal with any security to protect my database or the framework. In my case, .NET is pretty secure and I kept it up to date but what about a PHP backend? What about Ruby? Are you and your hoster applying all the patches? It's unsettling to think about that.

Second, it doesn't mattered how I generated my blog. I could as far as I know generate my site using FoxPro from a Windows XP VM and it would work. It doesn't mattter if the framework/language is dead because it's not exposed. This gave me some options.

Third, it doesn't matter where I host it. It could literally be served from a single Raspberry PI or [a cluster](https://www.hanselman.com/blog/HowToBuildAKubernetesClusterWithARMRaspberryPiThenRunNETCoreOnOpenFaas.aspx) and it would still work.

Finally, it makes changing platform easier. You're a Node fan today? Go use [Hexojs](https://hexo.io/) just like I did. Rather use Ruby? [Jekyll](https://jekyllrb.com/) is for you. Want to go with Go intead? Try [Hugo](https://gohugo.io/). 

That's what this blog post is about. It doesn't matter what you use. They all use the same files (mostly).

## Getting started

First, I'll take a copy of my blog. You could use yours if it's already in a static format but it's just going to be an example anyway. Mine is built with Hexojs. It doesn't matter unless you want to keep the template.

What is important is inside the folder `/source/_posts/`. This is where all the markdown files are.

I'll take a literal copy of my blog like so.

```bash
git clone https://github.com/MaximRouiller/blog.decayingcode.com
```

What we're going to do is generate a script to convert them over to two different engine in very different languages.

Impossible? A week long job? Not even.

Here are our goals. 
* [Wyam](https://wyam.io/) reboot
* [Hugo](https://gohugo.io) reboot

I'm not especially attached to my template so I exclude that from the conversion.

Also to take a note, images haven't been kept transitioned but it's a simple task to do too.

## Wyam Reboot

First, you'll need to [install Wyam's latest release](https://github.com/Wyamio/Wyam/releases).

I went with the zip file and extracted the content to `c:\tools\wyam`.

```powershell
# I don't want to retype the type so I'm temporarily adding it to my PowerShell Path
$env:Path += ";c:\tools\wyam";

# Let's create our blog
mkdir wyam-blog
cd wyam-blog
wyam.exe new --recipe blog

# Wyam create an initial markdown file. Let's remove that.
del .\input\posts\*.md

# Copying all the markdown files to the Wyam repository
cp ..\blog.decayingcode.com\source\_posts\*.md .\input\posts

# Markdown doesn't understand "date: " attribute so I change it to "Published: "
$mdFiles = Get-ChildItem .\input\posts *.md -rec
foreach ($file in $mdFiles)
{
    (Get-Content $file.PSPath) |
    Foreach-Object { $_ -replace "date: ", "Published: " } |
    Set-Content $file.PSPath
}

# Generating the blog itself.
wyam.exe --recipe Blog --theme CleanBlog

# Wyam doesn't come with an http server so we need to use something else to serve static files. Here I use the simple Node `http-server` package.
http-server .\output\
```
Sample URL: `http://localhost:8080/posts/fastest-way-to-create-an-azure-service-fabric-cluster-in-minutes`
![My blog in Wyam](/posts/files/static-site/wyam-blog.png)

## Hugo Reboot

First, you'll need to [install Hugo](https://gohugo.io/getting-started/installing).

Then select a theme. I went with the [Minimal theme](https://github.com.com/calintat/minimal/). 

```cmd
# Create a new hugo site and navigate to the directory
hugo new site hugo-blog
cd hugo-blog

# we want thing to be source controlled to create submodules
git init 

# selecting my theme
git submodule add https://github.com/calintat/minimal.git themes/minimal
git submodule init
git submodule update

# overwrite the default site configuration
cp .\themes\minimal\exampleSite\config.toml .\config.toml

# create the right directory for posts
mkdir .\content\post

# Copying all the markdown files to the Hugo repository
cp ..\blog.decayingcode.com\source\_posts\*.md .\content\post

# generate the site with the selected theme and serve it
hugo server -t minimal
```

Sample URL: `http://localhost:1313/post/fastest-way-to-create-an-azure-service-fabric-cluster-in-minutes/`

![My blog in Hugo](/posts/files/static-site/gohugo-blog.png)

## Result

So how long did it take me? Hours? No. In fact, it took me longer to write this blog post relating the events than doing the actual conversion.

## Why should I care about your blog?

Well you shouldn't. The blog is an excuse to talk about static content. 

Throughout our careers, we often get stuck in a pattern of using certain solutions for every problems we encounter. Sometimes, it's useful to disconnect ourselves from our hammers to stop seeing nails everywhere.

This is what it's about.

Now. Let's forget about blogs and take a look at your current project. Does everything need to be dynamic? Do you need to have your data generated every time? I'm not just talking HTML here. I'm also talking JSON, XML, etc.

What could be transformed from a dynamic pipeline linked to a language full of features and execution path to a simple IO operation?

Let me know in the comment.