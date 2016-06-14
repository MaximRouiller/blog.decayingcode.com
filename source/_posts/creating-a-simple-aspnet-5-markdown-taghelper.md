---
title: "Creating a simple ASP.NET 5 Markdown TagHelper"
date: 2015-11-03 12:00:00
tags: [asp.net]
---

I've been dabbling a bit with the new ASP.NET 5 TagHelpers and I was wondering how easy it would be to create one.

I've created a simple Markdown TagHelper with the CommonMark implementation.

So let me show you what it is, what each line of code is doing and how to implement it in an ASP.NET MVC 6 application.

### The Code

```csharp
using CommonMark;
using Microsoft.AspNet.Mvc.Rendering;
using Microsoft.AspNet.Razor.Runtime.TagHelpers;

namespace My.TagHelpers
{
    [HtmlTargetElement("markdown")]
    public class MarkdownTagHelper : TagHelper
    {
        public ModelExpression Content { get; set; }
        public override void Process(TagHelperContext context, TagHelperOutput output)
        {
            output.TagMode = TagMode.SelfClosing;
            output.TagName = null;

            var markdown = Content.Model.ToString();
            var html = CommonMarkConverter.Convert(markdown);
            output.Content.SetContentEncoded(html);
        }
    }
}
```

### Inspecting the code
Let's start with the `HtmlTargetElementAttribute`. This will wire the HTML Tag `<markdown></markdown>` to be interpreted and processed by this class. There is nothing stop you from actually having more than one target. 

You could for example target element `<md></md>` by just adding `[HtmlTargetElement("md")]` and it would support both tags without any other changes.

The `Content` property will allow you to write code like this:

```html
@model MyClass

<markdown content="@ViewData["markdown"]"></markdown>
<markdown content="Markdown"></markdown>
```

This easily allows you to use your model or any server-side code without having to handle data mapping manually.

TagMode.SelfClosing will force the HTML to use self-closing tag rather than having content inside (which we're not going to use anyway). So now we have this:

```html
<markdown content="Markdown" />
```

All the remaining lines of code are dedicated to making sure that the content we render is actual HTML. `output.TagName` just make sure that we do not render the actual `markdown` tag.

And... that's it. Our code is complete.

### Activating it

Now you can't just go and create TagHelpers and have them automatically served without wiring one thing.

In your ASP.NET 5 projects, go to `/Views/_ViewImports.cshtml`.

You should see something like this:

```razor
@addTagHelper "*, Microsoft.AspNet.Mvc.TagHelpers"
```

This will load all TagHelpers from the `Microsoft.AspNet.Mvc.TagHelpers` assembly.

Just duplicate the line and type-in your assembly name.

Then in your Razor code you can have the code bellow:

```csharp
public class MyClass
{
    public string Markdown { get; set; }
}
```

```html
@model MyClass
@{
    ViewData["Title"] = "About";
}
<h2>@ViewData["Title"].</h2>

<markdown content="Markdown"/>
```

Which will output your markdown formatted as HTML.

Now whether you load your markdown from files, database or anywhere... you can have your user write rich text in any text box and have your application generate safe HTML. 

### Components used
* [CommonMark.NET](https://github.com/Knagis/CommonMark.NET)
