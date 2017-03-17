---
title : "Adding TFS to your Powershell Command Line"
date: 2017-03-17 10:00
tags: [tfs, powershell]
---

If you are mostly working with other editors than Visual Studio but still want to be able to use TFS with your team mates, you will need a command line solution.

The first thing you will probably do is to do a Google Search and find where the command line utility is located.

> C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\tf.exe

Now, you could simply add the folder to your `%PATH%` and make it available to your whole machine. But... what about setting an alias instead? Basically, just import this specific command without importing the whole folder.

### PowerShell

First, run this `notepad $PROFILE`. This will open your PowerShell profile script. If the file doesn't exist, it will prompt you to create it.

Once the file is opened, copy/paste the following line:

```powershell
Set-Alias tf "$env:VS140COMNTOOLS..\IDE\tf.exe"
```

If you have a different version of Visual Studio installed, you may need to change the version of the common tools.

This is easily the easiest way to get Team Foundation Services added to your command line without messing with your `PATH` variable.

### Tools Versions

| Name                | Version | Tools Variable |
| ---                 | ---     | ---            |
| Visual Studio 2010  | 10.0    | VS100COMNTOOLS |
| Visual Studio 2012  | 11.0    | VS110COMNTOOLS |
| Visual Studio 2013  | 12.0    | VS120COMNTOOLS |
| Visual Studio 2015  | 14.0    | VS140COMNTOOLS |
| Visual Studio 2017  | 15.0    | VS150COMNTOOLS |


### Testing it out

If you run `tf get` on a source controlled folder, you should see changes be brought back to your folder.

Is there any other tools that you are using that are not registered in the default PATH? Leave a comment and let everybody know!
