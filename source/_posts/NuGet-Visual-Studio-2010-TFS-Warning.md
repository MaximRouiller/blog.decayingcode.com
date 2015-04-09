---
title: "NuGet + Visual Studio 2010 + TFS = Warning"
date: 2011-03-02 08:44:39
tags: [f8be83b8-613c-4336-8ddd-31e8439b8d47,c8b8089f-79b2-4e18-94a5-ebec0c09501d,f0c461c2-45b2-453e-b1af-7637f8e74fb0]
---

Someone asked me if adding a package through NuGet to a Source Controlled project would add the folder “packages” to TFS.

Well I got the answer today and it’s “No”.

Make sure you go inside Source Control Explorer and add that folder. Make sure to also include everything since DLLs are normally excluded by default.