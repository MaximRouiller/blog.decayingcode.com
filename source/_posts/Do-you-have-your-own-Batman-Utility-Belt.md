---
title: "Do you have your own Batman Utility Belt?"
date: 2014-11-11 23:22:43
tags: [tools,opinion]
---

Just like most of us on any project, you (yes you!) as a developer must have done the same thing over and over again. I'm not talking about coding a controller or accessing the database.

Let's check out some concrete examples shall we?

*   Have you ever setup HTTP Caching properly, created a class for your project and call it done?
*   What about creating a proper Web.config to configure static asset caching?
*   And what about creating a MediaTypeFormatter for handling CSV or some other custom type?
*   What about that BaseController that you rebuild from project to project?
*   And those extension methods that you use ALL the time but rebuild for each projects...

If you answered yes to any of those questions... you are in great risk of having to code those again.

Hell... maybe someone already built them out there. But more often than not, they will be packed with other classes that you are not using. However, most of those projects are open source and will allow you to build your own Batman utility belt!

So once you see that you do something often, start building your utility belt! Grab those open source classes left and right (make sure to follow the licenses!) and start building your own class library.

### NuGet

Once you have a good collection that is properly separated in a project and that you seem ready to kick some monkey ass, the only way to go is to use NuGet to pack it together!

[Checkout the reference](http://docs.nuget.org/docs/reference/command-line-reference#Pack_Command) to make sure that you do things properly.

### NuGet - Publishing

OK you got a steamy new hot NuGet package that you are ready to use? You can either push it to the main repository if your intention is to share it with the world.

If you are not ready quite yet, there are multiple way to use a NuGet package internally in your company. The easiest? Just create a Share on a server and add it to your package source! As simple as that!

Now just make sure to increment your version number on each release by using the [SemVer](http://www.semver.org/) convention.

### Reap the profit

OK, no... not really. You probably won't be money anytime soon with this library. At least not in real money. Where you will gain however is when you are asked to do one of those boring task yet over again in another project or at another client.

The only thing you'll do is import your magic package, use it and boom. This task that they planned would take a whole day? Got finished in minutes.

As you build up your toolkit, more and more task will become easier to accomplish.

The only thing left to consider is what NOT to put in your toolkit.

### Last minute warning

If you have an employer, make sure that your contract allows you to reuse code. Some contracts allows you to do that but double check with your employer.

If you are a company, make sure not to bill your client for the time spent building your tool or he might have the right to claim them as his own since you billed him for it.

In case of doubt, double check with a lawyer!