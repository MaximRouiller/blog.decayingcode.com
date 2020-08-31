---
title : "From classic Command Prompt to fully customizable Terminal"
date: 2020-08-31 11:30:00
tags: []
---

Do you remember the Command Prompt? Are you still using it?

![Command Prompt](/posts/files/windows-terminal/cmd.png)

There were so many customization options in there like font size, font type, and colors (all eight of them)!

![Command Prompt Options](/posts/files/windows-terminal/cmd-options.png)

If like me, you need a bit more customization, I do have something for you.

Windows Terminal is the re-imagination of what a first-class command prompt experience should and gosh does it deliver. Let me show you what I got when I first launched the Command Prompt from the terminal.

## Windows Terminal First Look around

![Introducing the Amazing Windows Terminal](/posts/files/windows-terminal/wt-cmd.png)

So the first thing I noticed? Transparency. I know it may sound superficial, but it's catchy. The second thing? The tab with two buttons beside it. `+` will allow me to have more Command Prompt in here without changing windows(yay!).

The down arrow had me wondering for a second, so I clicked it.

![So many options...](/posts/files/windows-terminal/wt-dropdown.png)

Your options may vary, but I have Windows Linux Subsystem installed on my machine and a few other options, so Ubuntu shows up. I'm amazed that I can use any of those prompts from a single option. Mind blown!

## Back to Command Prompt

We can open up any shell/prompt/command line from Windows Terminal, which is nice, but how does it make Command Prompt better?

See that image above with the `Setting` option in there? Let's click on that, and it opens up this file in Visual Studio Code.

## Making the old feel new again

![Settings.json of Windows Terminal](/posts/files/windows-terminal/wt-settings.png)

That's a ton of JSON, but it will become quite easy quite fast.

The first link at `Line 3` will bring you to the [Official Docs](https://docs.microsoft.com/windows/terminal/?WT.mc_id=educatordeveloper-blog-marouill), which is perfect. It will show you how to go in detail on every point.

I want to make this even more straightforward for you.

Do you see `Line 6`? Visual Studio Code will read the schema definition and enable code completion within your JSON file straight away.

If I wanted to modify something, I would create a new line and press `"`, and suddenly, you have all the options available.

Let me give you a new `cmd.exe` profile that you can overwrite and have something feels brand new right now.

```json
{
    // Make changes here to the cmd.exe profile
    "guid": "{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",
    "name": "cmd",
    "commandline": "cmd.exe",
    "useAcrylic": true,
    "acrylicOpacity": 0.7,
    "backgroundImageOpacity": 0.7,
    "backgroundImageStretchMode": "fill",
    "backgroundImage": "https://wallpapercave.com/wp/wp2053618.jpg",
    "startingDirectory": "C:\\git_ws\\",
    "fontFace": "Cascadia Code",
    "fontSize": 12,
    "hidden": false
}
```

## End Result

![Refreshed Command Prompt](/posts/files/windows-terminal/wt-cmd-refreshed.png)

That doesn't even closely look like our classic Command Prompt.

There are be many more options to cover that we can cover in another article. What we did for Command Prompt, we could do for every terminal/shell in our previous list.

## Next Steps

Want to have a terminal that stands out? Do you want a terminal that is suited just for you and no one else?

[Follow our installation instructions](https://docs.microsoft.com/windows/terminal/get-started?WT.mc_id=educatordeveloper-blog-marouill), and you will be started in less than 5 minutes.

After installing the Windows Terminal, start by copy/pasting settings from profiles you like in our [Custom Terminal Gallery](https://docs.microsoft.com/windows/terminal/custom-terminal-gallery/powerline-in-powershell?WT.mc_id=educatordeveloper-blog-marouill).

With those basics mastered, you are ready to tweak Windows Terminal until you feel at home with any shell.

If you create something unique, please [share it with me on Twitter](https://twitter.com/MaximRouiller)! I can't wait to see your creations!
