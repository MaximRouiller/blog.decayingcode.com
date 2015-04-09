---
title: "Javascript and CSS Minifying/Bundling with the Microsoft.Web.Optimization Nuget package"
date: 2012-01-23 06:47:14
tags: [javascript, asp.net]
---

So I’ve been wanting to write about this since the build and only gotten around to do it now.

When you write C# code, you rather have multiple small files with clear separation of concerns. This allow you to have small and clear classes and the compiler will never complain about it. However, in Javascript, you want to have smaller files. Most of the time in the .NET environment, there wasn’t any integrated way of doing so. Either it required an EXE call or outputing `.min.js` files. 

This caused problems as we had to alter our Development version of our HTML to fit our Production environment. Microsoft released this tid bit early because it’s probably going to be integrated in the .NET 4.5 framework but is making it available to us now. 

Please be aware that `Microsoft.*` DLLs are not part of the official framework and when they do, they will probably be changed namespace to `System.*`.

## Pre-requisites

First, you will need NuGet to install the following packages:

*   Microsoft.Web.Optimization
*   WebActivator  

## How it works

Now, the way the JS/CSS minifying works is that it will dynamically inspect all your files, read them, minify them and then cache the result to be served later. This allow us to modify our files and have all the files re-minified. When one of our JS/CSS file get modified again, this process will restart until either the cache expire or a file change.

## Setting up the base work

For the minify-er to work, it will require the registration of an HttpModule. It’s not already included in the Microsoft.Web.Optimization package but it will be necessary for us to add it if we want it to work.

```cs
using Microsoft.Web.Infrastructure.DynamicModuleHelper;
using Microsoft.Web.Optimization;
using MvcBackbonePrototype.Bundle;

[assembly: WebActivator.PreApplicationStartMethod(typeof(MvcBackbonePrototype.AppStart.BundleAppStart), "Start")]

namespace MvcBackbonePrototype.AppStart
{
    public static class BundleAppStart 
    {
        public static void Start()
        {
            DynamicModuleUtility.RegisterModule(typeof (BundleModule));
            RegisterFolders();
        }

        private static void RegisterFolders()
        {
            // configure Microsoft.Web.Optimization
        }
    }
}
```

The previous code will do the following, when your application start, it will register a dynamic HttpModule.

Now that the base work is done, we’ll jump right ahead to the configuration of the folders.

## Configuring the package

Now that the HttpModule is properly registered, we need to tell the Module when to activate itself. In my specific scenario, I wanted to have jQuery, underscore.js and Backbone.js in that specific order. 

By default, the Module will load most core frameworks first (jQuery, MooTools, prototype, scriptaculous) and then load the rest of the files that doesn’t match the wildcards after. The filters are done so that jQuery plugins will load after the jQuery core library and jQuery UI will load after jQuery.

However, there is nothing done for underscore.js and Backbone.js.


```cs
private static void RegisterFolders()
{
    var js = new DynamicFolderBundle("js", typeof(JsMinify), "*.js", false);
    BundleTable.Bundles.Add(js);
}
```

The previous code correctly configure the module to minify all files in a folder by just adding the suffix “js” to the folder (eg.: /Scripts/js).

However, it will register the the other modules in alphabetical order rather than the proper order. 

Let’s fix that.

## Custom Orderer

```cs
public class BackboneOrderer: DefaultBundleOrderer
{
    public override IEnumerable<FileInfo> OrderFiles(BundleContext context, IEnumerable<FileInfo> files)
    {
        context.BundleCollection.AddDefaultFileOrderings();

        var backboneOrdering = new BundleFileSetOrdering("backbone");
        backboneOrdering.Files.Add("underscore.*");
        backboneOrdering.Files.Add("backbone.*");
        context.BundleCollection.FileSetOrderList.Add(backboneOrdering);

        return base.OrderFiles(context, files);
    }
}
```

We first inherit from the default order. Then, we add the default file ordering which will take care of the jQuery ordering for us. Then, we add the other files that we require to the list. The only thing left is to alter our RegisterFolders method to fix that.

```cs
private static void RegisterFolders()
{
    var js = new DynamicFolderBundle("js", typeof(JsMinify), "*.js", false);
    js.Orderer = new BackboneOrderer();
    BundleTable.Bundles.Add(js);
}
```

That’s it. We are nearly done!

Modifying your _Layout.cshtml / masterpage

My masterpage head section first looked a lot like this:


```html
<script src="@Url.Content("~/Scripts/Framework/jquery-1.7.1.min.js")" type="text/javascript"></script>
<script src="@Url.Content("~/Scripts/Framework/underscore.min.js")" type="text/javascript"></script>
<script src="@Url.Content("~/Scripts/Framework/backbone.min.js")" type="text/javascript"></script>
```

This was of course replaced by the following:


```html
<script src="@Url.Content("~/Scripts/Framework/js")" type="text/javascript"></script>
```

And that’s all! All your files will be minimized, bundled and properly cached.

## Bonus

If you want to have your URLs with a “version number” on it, I suggest that you use the following methods to resolve your URLs instead of the MVC way:


```html
<script src="@Microsoft.Web.Optimization.BundleTable.Bundles.ResolveBundleUrl("~/Scripts/Framework/js", true)"></script>
```