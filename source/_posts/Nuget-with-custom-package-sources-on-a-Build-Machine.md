---
title: "Nuget with custom package sources on a Build Machine"
date: 2013-05-14 09:55:00
tags: [build,nuget]
---

My build was failing due to moving to a new build machine. We do have custom packages that are used internally and they could not be found by NuGet on the new server.

This post might be more about me remembering where it is but the location of the package sources for NuGet is found here:

```text
%APPDATA%\NuGet\NuGet.Config
```

Here is what mine looks like:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageRestore>
    <add key="enabled" value="True" />
  </packageRestore>
  <packageSources>
    <add key="NuGet official package source" value="https://nuget.org/api/v2/" />
    <add key="LocalFeed" value="C:\LocalPackages\" />
    <add key="MyCompany" value="\\myServer\NugetPackages" />
  </packageSources>
  <disabledPackageSources />
  <activePackageSource>
    <add key="NuGet official package source" value="https://nuget.org/api/v2/" />
  </activePackageSource>
</configuration>
```

The same file can be found on the build server (with some options missing). The main part is the packageSources section.

If you are depending on a local repository or a different repository than the public NuGet Gallery, it&rsquo;s where you need to update your sources to have your package available.