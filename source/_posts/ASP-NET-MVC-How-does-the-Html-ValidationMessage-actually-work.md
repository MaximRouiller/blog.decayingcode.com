---
title: "ASP.NET MVC - How does the Html.ValidationMessage actually work?"
date: 2009-12-10 11:53:00
tags: []
---

When you create a basic ASP.NET MVC application, you normally will have "Html.ValidationMessage" inserted automatically for you in the Edit and Create views. Of course, if you try to type strings in a number field, it will fail. Same things for dates and such. The good question now is... how does it do it?

Well, the ValidationMessage method only look to see if the model you gave him with the name given have received errors. If it did, it will display the specified message. So now that we covered the "How", I'll show you where it does that.

The answer lies within the DefaultModelBinder that comes activated by default with ASP.NET MVC. The ModelBinder do a best guest to fill your model with the values sent from a post. However, when it can match a property name but can't set the value (invalid data), it will catch the exception and add it as an error in a ModelStateDictionary. The ValidationMessage then picks up data from that dictionary and will find errors for the right property of your model.

That's it! Of course it's pretty simple validation and I would still recommend you to use a different validation library. There is already a few available on the [MVCContrib](http://mvccontrib.codeplex.com/documentation?referringTitle=Home "Home") project on CodePlex.
