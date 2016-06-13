---
title: "It's the perfect time for breaking changes in .NET"
date: 2016-06-13 16:00:00
tags: [.net,architecture]
---

Breaking changes is something that happens all the time in the Open Source Community.

We see people going from [Grunt to Gulp][1] because the new way of doing things is better. In the open-source world, project live or dies based on their perceived value.

In the .NET World, things are more stable. Microsoft ensures backward compatibility on their language and frameworks for years. People get used to seeing the same technology around and, with this, no reasons to change things since it's going to be supported for sometimes decades.

Microsoft adoption of OSS practices, however, changed their approach to software. To become faster, things needed to be broken down and rebuilt. Changes needed to happen. To build a framework ready to support to fast-paced change of tomorrow, things we were used to are being ripped apart and rebuilt from scratch.

Not everything was removed. Some good concepts were kept, but it opened the door to changes.

### Being open to change

This is the world we live in. I don't know if it's Microsoft's direction, but we need to be able to stay open to change even if it breaks our stuff. Microsoft is a special island where things stay alive for way longer than sometimes they should.  

In ASP.NET Core, they went so far that they had to revert some changes to be able to deliver.

### Good or bad?

Here's my opinion on the matter. Things that change too quickly can be bad for your ecosystem because people can't find their footing and spend more time finding out what broke than delivering value.

Change is necessary. Otherwise, you end up like Java and have [this monstrosity][2] when handling date. C# isn't too different in the collection department either.

### What the future will look like

I don't know what the team is planning. What I hope is that dead part of the framework are retired as newer versions of the framework are released.

It's time to move the cheese and throw the dead weight overboard. Otherwise, you're just dragging it along for the next 10 years.

[1]: http://www.100percentjs.com/just-like-grunt-gulp-browserify-now/
[2]: http://stackoverflow.com/questions/1404210/java-date-vs-calendar
