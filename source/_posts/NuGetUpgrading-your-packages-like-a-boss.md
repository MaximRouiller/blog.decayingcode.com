---
title: "NuGet–Upgrading your packages like a boss"
date: 2014-08-11 16:45:17
tags: [nuget]
---

How often do you get on a project and just to assess where are things… you open the “Manage NuGet Packages for Solution…” and go to the Updates tab.

Then… you see this.

[![image](http://blog.decayingcode.com/posts/files/010e108a-8c5d-446e-8236-d66baf5127fd.png "image")](http://blog.decayingcode.com/posts/files/b2c1e487-3473-478f-8171-4ceaa527de12.png)

I mean… there’s everything in there. From Javscript dependencies to ORM. You know that you are in for a world of trouble.

### Problem

You see the “Update All” and it’s very tempting. However, you know you are applying all kinds of upgrades. This could be fine when starting a project but when maintaining an existing project… you are literally including new features and bugs fixes for all those libraries.

A lot can go wrong. 

### Solution A: Update All a.k.a. Hulk Smash

So you said… screw it. My client and me will live with the consequences. You press Update All and… everything still works on compile.

Congratulation! You are in the very few! 

Usual case? Compile errors everywhere that you will need to fix ASAP before committing. 

Worse case? Something breaks in production and it takes us to this:

[![code_test_meme](http://blog.decayingcode.com/posts/files/cfd8e201-b3b0-48f0-8b82-affdf6e9ad54.jpg "code_test_meme")](http://blog.decayingcode.com/posts/files/e14f87dd-fcdc-4e56-bfd1-833918233790.jpg)

### Solution B: Update safely a.k.a The Boss Approach 

Alright… so you don’t want to go Hulk Smash on your libraries and on your code. And more importantly, you don’t want to be forced to wear the cowboy hat for a week.

So what is a developer to do in this case? You do it like a boss.

First, you open up “View &gt; Other Windows &gt; Package Manager Console”. Yes. It’s hidden but it’s for the pro. The kings. People like you who don’t use a tank to kill a fly.

It will look like this: 

[![image](http://blog.decayingcode.com/posts/files/3858e876-2a72-4b15-8a82-d20f8f5bc5fb.png "image")](http://blog.decayingcode.com/posts/files/b1525815-df40-4567-b016-b01b8588dc74.png)

What is this? This beauty is Powershell. Yes. It’s awesome. There’s even [a song about it](https://www.youtube.com/watch?v=Z42M8GT4lSc). 

So now that we have powershell… what can we do? Let me show you to your scalpel boss. 

[Update-Package](http://docs.nuget.org/docs/reference/package-manager-console-powershell-reference#Update-Package) is your best friend for this scenario. Here is what you are going to do:

```ps
Update-Package -Safe
```

That’s it.

### What was done

This little “Safe” switch will only upgrade Revisions and will not touch Major and Minor versions. So to quote the documentation:

> The '-Safe' flag constrains upgrades to only versions with the same Major and Minor version component.

That’s it. Now you can recompile your app and most of your app should have all bug fixes for current Major+Minor versions applied.

[![like-a-boss](http://blog.decayingcode.com/posts/files/4c5dc1e5-8454-4f8e-b328-4fe70c0ba56f.jpg "like-a-boss")](http://blog.decayingcode.com/posts/files/29bc51b6-92bb-4647-b51f-776d96fad2bd.jpg)

If you want to read more about Semantic Versioning (which is what NuGet uses), go read [Alexandre Brisebois’ post](http://alexandrebrisebois.wordpress.com/2014/08/11/how-do-you-version-packages/) on it. Very informative and straight to the point.
