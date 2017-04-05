---
title : "What's new in .NET Core 1.0? - New Project System"
date: 2017-04-05 13:00
tags: [c#, visual studio, .net core]
---

There's been [a lot of fury](https://github.com/aspnet/Home/issues/1433) from the general ecosystem about retiring `project.json`.

It had to be done if Microsoft didn't want to work in parallel on 2 different build systems with different ideas and different maintainers.

Without restarting the war that was the previous GitHub issue, let's see what the new project system is all about!

### csproj instead of project.json

First, we're back with csproj. So let's simply create a single .NET Core Console app from Visual Studio that we'll originally call `ConsoleApp1`. Yeah, I'm that creative.

### Editing csproj

By right clicking on the project, we can see a new option.

![Editing csproj](/posts/files/core-build-system/editing-csproj.png)

Remember when opening `csproj` before? You could either have the solution loaded or edit the `csproj` manually. Never at the same time. Of course, we would all open the file in Notepad (or Notepad++) and edit the file anyway.

Once we came back to Visual Studio however, we were prompted to reload the project. This was a pain.

No more.

![Editing csproj](/posts/files/core-build-system/editing-csproj-2.png)

Did you notice something?

### New csproj format

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

</Project>
```

Yeah. It is. The whole content of my `csproj`. Listing all your files is not mandatory anymore. Here's another thing, open Notepad and copy/paste the following into it.

```csharp
public class EverythingIsAwesome
{
    public bool PartOfATeam => true;
}
```

Now, save that file (mine is `Test.cs`) at the root of your project. Right beside `Program.cs` and swap back to Visual Studio.

![Everything is awesome](/posts/files/core-build-system/editing-csproj-3.png)

That's the kind of features my dreams are made of. No more having to `Show all files`, then include external files into my projects, then resolving all those merge conflicts.

### Excluding files

What about the file you don't want?

While keeping the `csproj` open, right click on `Test.cs` and exclude it. Your project file should have this added to it.

```xml
<ItemGroup>
  <Compile Remove="Test.cs" />
</ItemGroup>
```

What if I want to remove more than one file? Good news everyone! It supports wildcards. You can remove, single file, folders and more.

Now remove that section from your `csproj`. Save. `Test.cs` should be back in your solution explorer.

### Are you going to use it?

When new features are introduced, I like to ask people whether it's a feature they would use or if it will impact their day to day.

So please leave me a comment and let me know if you want me to dig deeper.
