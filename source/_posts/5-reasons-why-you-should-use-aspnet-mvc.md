---
title: "5 reasons why you should use ASP.NET MVC"
date: 2009-04-05 01:32:00
tags: []
---

I'll be fair with you readers. I've only toyed with the ASP.NET MVC framework. It looks great as of now but it's the first full blown MVC framework that we have that is backed by Microsoft. However, there is a lot of opposition nowadays that tend to be formulated like this:

> Why should I use ASP.NET MVC? WebForms works well.

Other problems come from the lack of server controls. When a developer look at that and he then wonder why he should have to write HTML and Javascript when before he could have retrieved all that beautiful information with a simple postback.

So without ranting any further, here is 5 reasons why you should use ASP.NET MVC.

#### 1\. Testability

When the MVC model is properly applied, it allow for a better separation of your business logic and your presentation code. If the view is not included inside your model, you can easily test without requiring a web server. By default, when starting a new MVC project, Visual Studio offer to create a new Unit Test project based on Microsoft's Unit Test framework. Other Unit tests framework can also be configured to be used by default instead of Microsoft's solution.

The way the code is also made, the controller is the one handling the calls from the route. They can be instantiated outside of a web request which makes them easy to test too.

#### 2\. Perfect control of the URLs

ASP.NET MVC use URL Routing to better control the request and forward them to your controllers. Instead of 1 to 1 mapping, they allow pattern matching. The default being "{controller}/{action}/{id}" with the default being "Home/Index". This technically allow you to set the URLs exactly how you want. You don't have to create folder for every level deep it goes. The URL routing allows you to make clean URL that will be easy to remember.

Would you rather try to remember http://localhost/Sales/DisplayProduct.aspx?ProductID=23213 or http://localhost/Product/Detail/23213 ? Even better, if you are an e-Commerce site and want some fast link.... you can directly bind those URL to http://localhost/23213 to make it more easy to remember. Doing that in WebForm while keeping all this unit testable would just be too time consuming now is it?

#### 3\. Better Mobility Support

In WebForm, you would have to detect on each page that the browser is a mobile and adapt your rendering for the mobile on each and every form. You could also redirect the user to different page when it's a mobile. What is excellent with MVC is that it's not the view that is receiving the request. It's the controller. The controller can then dynamically decide which view to render while keeping the same URL. So to see a product view, you don't even need to send different URL to different provider. You just [detect which device you are handling and redirect it to the proper view](http://www.hanselman.com/blog/MixMobileWebSitesWithASPNETMVCAndTheMobileBrowserDefinitionFile.aspx). As you support more and more mobile device, you can keep on adding view that are more specific to each device. Want to support this new HTC? Create a view, detect the browser and ensure the right view is displayed. Want to support some iPhone goodness with some device specific HTML? Create the necessary view, reuse the browser detection and display the view.

You can keep on doing that ad infinitum and as much as you want depending on your audience. Having Mobile support now is more convenient than it has ever been.

#### 4\. View Engines

Now if you only built ASP.NET WebForms, this term might be weird for you. Let's just say that you have been using the same view engine all this time without wondering if you could choose. The WebForm view engine is... well... what you have been using all this time. This includes server tags (&lt;% %&gt;), binding tags (&lt;%# %&gt;) as well as control tag (&lt;asp:TextBox ... /&gt;).

The [Spark Engine](http://sparkviewengine.com/) is a good example. [MvcContrib](http://www.codeplex.com/MVCContrib) also offer 4 different view engine ([Brail](http://mvccontrib.codeplex.com/Wiki/View.aspx?title=Brail&amp;referringTitle=Documentation "Brail&amp;referringTitle=Documentation"), [NHaml](http://code.google.com/p/nhaml/), NVelocity, XSLT). Each of those engine are created to fix some specific problems. Different view engines can be used on different view. One page could be handled with WebForm view engine, one with Spark Engine, one with XSLT, etc. Different view, different problem different solution.

You might not have to use those, but the simple fact that they are available will make your life easier if they are needed.

#### 5\. Built-in and shipped jQuery support

Let's keep the best for the end. jQuery is shipped with any new project instance of ASP.NET MVC. Since Microsoft announced support for jQuery, it's been the big buzz in the javascript world. Since ASP.NET MVC don't rely on Postback, a strong javascript framework is needed to provide for all the UI the previous server control were offering. jQuery easily offer you AJAX, DOM manipulation, event binding and&nbsp; this across browser.

Of course, jQuery is not an advance to MVC itself. But it is a serious offering from ASP.NET MVC. No more download or "I'll write my own" stuff. I don't know for all of you but if I have to do javascript, I normally do a _document.getElementById_. This will work in most browsers but as soon as you start going funky, some browser will misbehave. jQuery simply allow you to write _$("#myControlId")_ or many more shortcuts to simply do what you need across browsers. Just by having jQuery available stops me from writing incompatible code.

#### Conclusions

Lots of point goes toward MVC. Way more could be added. You certainly don't want to miss [Kazi Manzur Rashid](http://weblogs.asp.net/rashid/default.aspx)'s blog about ASP.NET MVC Best Practices ([part 1](http://weblogs.asp.net/rashid/archive/2009/04/01/asp-net-mvc-best-practices-part-1.aspx), [part 2](http://weblogs.asp.net/rashid/archive/2009/04/03/asp-net-mvc-best-practices-part-2.aspx)). [Scott Hanselman](http://www.hanselman.com/), [Phil Haack](http://www.haacked.com) also have great posts about ASP.NET MVC.

Don't be fooled. Web Forms are not necessarly evil. They just aren't leading you to a [pit](http://blogs.msdn.com/brada/archive/2003/10/02/50420.aspx) [of](http://www.codinghorror.com/blog/archives/000940.html) [success](http://brandonr.mbablogs.businessweek.com/archive/2007/10/14/the-pit-of-success.htm).
