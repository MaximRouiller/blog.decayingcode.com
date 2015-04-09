---
title: "Which bug reporting method should I choose?"
date: 2009-04-13 23:34:00
tags: []
---

Alright, my previous post was lot of ranting... I have to make it up. I hate ranting because it never bring any solutions, only problems. When someone is building a software, bugs will happen as true as the sun rise everyday. Sometimes it will be simple change request but there will be bug that are going to be logged in.

It's important that the user of the software has an easy way of reporting the bug. Depending on the kind of software you are building, there will be different kind of way to actually report a bug. There is 4 main kind of software that programmer works on. Open-source software, product, internal software and public web sites. They all require different kind of bug reporting due to their reach and needs.

#### Open-source software

Open-source software are normally better with 2 kind of bug reporting. The first is an actual bug reporting tool that is linked to a source control software like [Google Code](http://code.google.com) or [Codeplex](http://www.codeplex.com). The other kind is a discussion list/forum. The first one is to actually report bug and the second one refer more to actual support by the community. It might sound confusing but they both highlight bugs as the consumer have a way to contact the project leader or contributors to the project.

Examples includes: [Firefox](https://bugzilla.mozilla.org/), [Moq](http://code.google.com/p/moq/ "Moq"), [BlogEngine.NET](http://blogengine.codeplex.com/)

#### Product

Product are either websites or actual executables that are sold to consumers. Since they don't display the source and especially not their bug lists, they need a way for the consumer to actually report their bugs. Since products are normally backed by a company with staffing, bug reporting can go from the simplest form to a complicated bug submission tools. Sometimes, the integration of the bug report are even more advanced. Some software even catch crash in the software and send them when the software can successfully restart. Whatever the way the bugs are caught, company should always offer a free way of sending bug report even if it's through a [support@example.com](mailto:support@example.com) kind of email.

Examples include: [FogBugz](http://www.fogcreek.com/sendmail.html), [Copilot](http://feedback.copilot.com/pages/general), [StackOverflow](http://stackoverflow.uservoice.com/pages/general), [Apple Mac OSX](http://www.apple.com/feedback/macosx.html)

#### Internal Software

Those are whole different kind of game. Those are software that you are hired to build for another company or that you build for your own company. Those kind of software mostly stay internal and are never going to be released into the wild. Those includes, CMS, ERP, wiki, etc. Those are normally what most developer will face. Direct contact with the user about the bugs, Team System for the bug reporting (or any other kind of bug reporting software used internally). The bug list is internal and new bugs are often reported by a direct email. phone call or a conversation with the program manager/architect/team lead/developer.

#### Websites

Websites normally don't have specific bug reporting system. Most websites works or the user leaves. The main bug reporting system in those system is heavy logging for any errors that happens and IIS logs for anything that doesn't go through the .NET runtime. However, some websites that offer service might offer some way to report bugs but they are&nbsp; the exception... not the rule.

Examples include: [StackOverflow](http://www.stackoverflow.com)

#### Conclusion

Bug reporting is not what you want your user to use (unless you are selling bug reporting software). However, when a user is seeking to you to report a bug, you must offer them a painless process to report bugs. Without this, you will only be able to find out bugs from other forums where people are complaining(if you have a large user base) but mostly, you will never get any notice that the software doesn't work as planned.

So please... if I want to report a bug... make my life easy and make it a process that don't take more than a few clicks.

Thanks

