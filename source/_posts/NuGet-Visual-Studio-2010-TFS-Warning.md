---
title: "NuGet + Visual Studio 2010 + TFS = Warning"
date: 2011-03-02 08:44:39
tags: [nuget,visual studio,tfs]
---

Someone asked me if adding a package through NuGet to a Source Controlled project would add the folder “packages” to TFS.

Well I got the answer today and it’s “No”.

Make sure you go inside Source Control Explorer and add that folder. Make sure to also include everything since DLLs are normally excluded by default.
