---
title : "Contributing to Open-Source - My first roslyn pull request - Getting the environment ready"
date: 2017-04-04 12:30:00
tags: [open source, c#,  .net, visual studio]
---

I've always wanted to contribute back to the platform that constitute the biggest part of my job.

However, the platform itself is huge. Where do you start? Everyone says *do documentation*. Yeah. I've done that in the past. Those are easy pick. Everyone can do them. I've wanted to contribute real actual code that would help developers.

### Why now?

I had some time around lunch and I saw this tweet by [Sam Harwell](https://twitter.com/samharwell):

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Here&#39;s a piece of low-hanging fruit if someone wants to try contributing to <a href="https://twitter.com/roslyn">@roslyn</a> for the first time: <a href="https://t.co/3oEwAjzaQm">https://t.co/3oEwAjzaQm</a></p>&mdash; Sam Harwell (@samharwell) <a href="https://twitter.com/samharwell/status/848907600905326594">April 3, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Low-hanging fruit. Compiler. Actual code.

What could possibly go wrong?

### The Plan

The plan was relatively simple. I can't contribute if I can't get the code compiling on my machine and the test running.

So the plan went as such:

1. Create a fork of [roslyn](https://www.github.com/dotnet/roslyn).
2. `git clone` the repository to my local machine
3. Follow the [instructions of the repository](https://github.com/dotnet/roslyn/wiki/Building%20Testing%20and%20Debugging). Well... the [master branch instructions](https://github.com/dotnet/roslyn/blob/master/docs/contributing/Building,%20Debugging,%20and%20Testing%20on%20Windows.md)
4. Test in Visual Studio
5. Create a pull request

### Getting my environment ready

I have a Surface Book i7 with 16Gb of RAM. I also have Visual Studio 2017 Enterprise installed with the basic .NET and WebDev workload installed.

#### Installing the necessary workloads

The machine is strong enough but if I wanted to test my code in Visual Studio, I would need to install the `Visual Studio extension development` workload.

This can easily be installed by opening the new [Visual Studio Installer](/post/whats-new-in-vs2017-visual-studio-installer/).

#### Forking the repository

So after installing the necessary workloads, I headed to GitHub and went to the [roslyn](https://www.github.com/dotnet/roslyn) repository and created a fork.

![Forking Roslyn](/posts/files/first-roslyn-pr/forking-roslyn.png)

#### Cloning the fork

You never want to clone the main repository. Especially in big projects. You fork it, and submit pull request from the fork.

So from my local machine I ran `git clone` from my fork:

![Cloning Roslyn](/posts/files/first-roslyn-pr/cloning-roslyn.png)

What it looks like for me:

```none
git clone https:github.com/MaximRouiller/roslyn.git
```

This may take a while. Roslyn is massive and there is ton of code. Almost 25k commits by the time of this post.

#### Running the Restore/Build/Test flow

That's super simple. You go to the roslyn directory with a prompt and you run the following command sequentially.

* `Restore.cmd`
* `Build.cmd`
* `Test.cmd`

If your machine doesn't use 100% CPU for a few minutes, you may have a monster of a machine. This took some a time. Restore and build were not too slow but the tests? Oh god... it doesn't matter what CPU you have. It's going down.

As I didn't want to spend another 30 minutes re-running all the tests, Sam Harwell suggested to use the `xunit.runner.wpf` to run specific tests as to avoid re-running the world. Trust me. Clone and build that repository. It will save you time.

And that completes the **Getting the environment ready**.

Time to actually fix the bug now.

### Fixing the bug

Stay with us for [part 2](/post/contributing-to-open-source-my-first-roslyn-pull-request-fixing-the-bug/) where we actually get to business.
