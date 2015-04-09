---
title: "Newbie debut with Sublime Text 2"
date: 2014-04-17 14:30:00
tags: [tool]
---

So I’ve started using Sublime Text 2 for some JavaScript fun. I’ve never found the tooling inside Visual Studio that great for JavaScript (TypeScript however is a whole other game) and since I’m going to spend quite some time editing JS files, I decided to give Sublime a go. 

The first time I heard from Sublime was at the MVP Summit in February 2013\. There was one big proponent of Sublime and one big room that wasn’t that interested. Things moved on but the name was there.

Today, I’m hearing more and more about Sublime and the community around it has grown incredibly.

## My pains so far…

First, the shortcut. I’m used to Visual Studio + ReSharper and oh god… the shortcuts are killing me. My brain is hardwired since at least 3-4 years in using ReSharper + VS shortcuts and to be honest… it’s the most horrible experience I have so far. However, I know that shortcut are mainly muscle memory. You think of an operation and your body knows exactly what to press without you even thinking about it. Rename? Boom. My fingers are on CTRL-R before my brain can remember the exact shortcut. This I know is going to be a world of hurt for me until my stupid brain get used to this.

Since we’re talking shortcuts…

## Surviving Sublime with some shortcuts

With ReSharper, you can get very far with CTRL-ENTER (fix whatever is where I am) and CTRL-SHIFT-R (refactor). 

So I was looking for something similar with Sublime. 
 <table style="border-top: black 1px solid; border-right: black 1px solid; border-bottom: black 1px solid; border-left: black 1px solid" cellspacing="0" cellpadding="2" width="624" border="2"> <tbody> <tr style="border-top: black 1px solid; border-right: black 1px solid; border-bottom: black 1px solid; border-left: black 1px solid"> <td valign="top" width="133">**Shortcut**</td> <td valign="top" width="133">**Action**</td> <td valign="top" width="354">**Description**</td></tr> <tr style="border-top: black 1px solid; border-right: black 1px solid; border-bottom: black 1px solid; border-left: black 1px solid"> <td valign="top" width="133">CTRL-P</td> <td valign="top" width="133">Quick-Open files</td> <td valign="top" width="354">Allows you to type file names with paths to allow you to get anywhere. Supports PascalCasing and partial match.</td></tr> <tr style="border-top: black 1px solid; border-right: black 1px solid; border-bottom: black 1px solid; border-left: black 1px solid"> <td valign="top" width="133">CTRL-SHIFT-R</td> <td valign="top" width="133">Go to Symbol</td> <td valign="top" width="354">Gets you to a recognized symbol. Symbols can be created by custom packages (more on that later). Sublime will find symbols in your files.</td></tr> <tr style="border-top: black 1px solid; border-right: black 1px solid; border-bottom: black 1px solid; border-left: black 1px solid"> <td valign="top" width="133">CTRL-SHIFT-P</td> <td valign="top" width="133">Command prompt</td> <td valign="top" width="354">This one is the power tool you are going to use once you get used to Sublime. It allows you to access commands from packages. </td></tr></tbody></table>For a more complete list of shortcuts, I recommend [going here](http://sublime-text-unofficial-documentation.readthedocs.org/en/latest/reference/keyboard_shortcuts_win.html). 

## Sublime and its packages

If like me you are coming from a Visual Studio environment, you should know NuGet. Well, Sublime has its equivalent but for the editor itself. 

[Click here](https://sublime.wbond.net/installation) for the instructions on getting Sublime ready to install packages. It will give you instructions on getting it ready.

So what can packages do? They can add supports for new languages, help the IDE find new Symbols, add snippets per languages, add commands, etc. A package can potentially do anything inside your project.

Some plugins like [LiveReload](https://sublime.wbond.net/packages/LiveReload) will automatically refresh your browser when you save a file. The [Git](https://sublime.wbond.net/packages/Git) plugin will add git commands to the CTRL-SHIFT-P interface so that you don’t even have to leave your editor to git commit. You like the TODO list in Visual Studio? [TodoReview](https://sublime.wbond.net/packages/TodoReview) generates a list of all todos in your project in any languages.

If there is something you want to do… there’s probably a Sublime Package for it.

[Head here to browse more packages](https://sublime.wbond.net/)

### Sublime Commands

Okay those are completely crazy once you start using them but is still quite a relatively simple concept. 

First, the commands are contextual to your file. So there won’t be any snippet for C# files if you are in JavaScript file. Then, some commands (like Package Control) are global and accessible wherever you are. The same “PascalCase” partial matching algorithm here is used for commands.

As an example, I installed a [JSLint](https://sublime.wbond.net/packages/JSLint) package and suddenly, I have a **JSLint** command available that I can run on any files.

### Conclusion

Okay so I’m still not “production ready” yet but if you have recommendation on which packages I should install (I’m a C#, ASP.NET, developer that does Javascript, Durandal, Angular, Knockout, etc.), please let me know in the comments!