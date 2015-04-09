---
title: "How to add custom build step to a TFS Server Build ?"
date: 2009-02-20 13:51:00
tags: [tfs]
---

Most of the time when you are creating a build script (TFSBuild.proj), you need to do some steps after the build. Wether it's creating an MSI for easier deployment, creating a VSI for a Visual Studio Add-in, or whatever if may be... you normally do a post build.

A post build event looks like the following inside the TFSBuild.proj :

```xml
<Target Name="AfterDropBuild">
    <CallTarget Targets="PostBuildStep" />
</Target>

<Target Name="PostBuildStep">
    <!-- Do something -->
</Target>
```

When you only have 1 or 2 tasks and that one fails, it might be easy to find the one that failed. What if you have 8 to 20 tasks? It then becomes incredibly hard to find which one failed. What I've seen the most is normally some "<Message />" tags with some descriptive tasks. This is the equivalent of debugging with Console.WriteLine or Debug.Print.

What if you could know EXACTLY which task failed to run? Here is a way to add a custom build step to your TFS build which will allow you to easily know what crashed.

```xml
<Target Name="PostBuildStep">
    <!-- Create the build steps which start in mode "Running" -->
    <BuildStep TeamFoundationServerUrl="$(TeamFoundationServerUrl)" BuildUri="$(BuildUri)" Message="Doing Something on a PostBuild Event" Condition=" '$(IsDesktopBuild)' != 'true' ">
    <!-- Return the ID of the tasks and assigned it to "PropertyName" -->
    <Output TaskParameter="Id" PropertyName="PostBuildStepId" />
    </BuildStep>

    <!-- Do something -->

    <!-- When everything is done, change the status of the task to "Succeeded" -->
    <BuildStep TeamFoundationServerUrl="$(TeamFoundationServerUrl)" BuildUri="$(BuildUri)" Id="$(PostBuildStepId)" Status="Succeeded" Condition=" '$(IsDesktopBuild)' != 'true' " />

    <!-- If an error occurs during the process, run the target "PostBuildStepFailed" -->
    <OnError ExecuteTargets="PostBuildStepFailed" />
</Target>

<Target Name="PostBuildStepFailed">
    <!-- Change the status of the task to "Failed" -->
    <BuildStep TeamFoundationServerUrl="$(TeamFoundationServerUrl)" BuildUri="$(BuildUri)" Id="$(PostBuildStepId)" Status="Failed" Condition=" '$(IsDesktopBuild)' != 'true' " />
</Target>
```

With that in place, you will see exactly which task failed. As a bonus, it will also give you the time at which it completed. This will easily allow you to compare with your other task to see which one is taking the most time.

I would like to thank [Martin Woodward](http://www.woodwardweb.com/) which is a TeamSystem MVP. The [question originated from Stackoverflow](http://stackoverflow.com/questions/226717/how-can-we-display-a-step-inside-visual-studio-build-process) and more details are also available on Martin's website.