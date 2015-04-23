---
title: "Find and Reproduce Heisenbugs - CHESS is the tool for you"
date: 2009-05-04 23:47:00
tags: []
---

Microsoft is mainly known for 2 things. Windows and Office. However, for programmers,&nbsp; Microsoft is also know for many more projects/product like .NET, Enterprise Library, ASP.NET MVC, Team Foundation Server, SharePoint, etc.

Among the few tools that are not really known and publicised at the moment are the projects inside Microsoft Research. This is the land of Beta or "software-never-to-be-released". Either the project is too crazy(read: inovative?) or not useful for people at the time. But there is time where a tool stands out and need to be&nbsp; talked about.

My friend [Eric De Carufel](http://blog.decarufel.net) recently talked to me about this tool called [CHESS](http://research.microsoft.com/en-us/projects/chess/) from Microsoft Research. He tested it and it allows for testing for those rare bugs that are only found when there is enough concurrency. Those bugs were tested before with stress test. Stress test are not supposed to be used for finding concurrency bugs but they are traditionally used for that too because when a system is under stress, concurrency happens.

CHESS is there to solve this problem. It includes itself among your unit test and try every possible permutation of code that happens asynchronously. This include race condition, dead lock, and what else exists (not an expert in multi-threaded applications).

Of course, I expect Eric to blog more about this amazing software that is coming from Microsoft's Trenches.

Want more information? Get on the [CHESS website](http://research.microsoft.com/en-us/projects/chess/doc.aspx). If you want to find more interesting projects that Microsoft's genius are working on, visit [Microsoft Research](http://research.microsoft.com/en-us/default.aspx).
