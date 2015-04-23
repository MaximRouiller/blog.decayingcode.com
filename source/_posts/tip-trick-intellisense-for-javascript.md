---
title: "Tip & Trick : Intellisense for Javascript in Visual Studio 2010"
date: 2011-01-31 12:40:00
tags: []
---

I know pretty much everyone already knows that but I would love to remember everyone how to get Intellisense working in Visual Studio 2010.

I mainly use jQuery and the API is huge. A bit huge to remember sometimes and I always have the documentation opened in a browser window. To alleviate my pain, I love to use the Intellisense and here is how you get it to work.

First, open your javascript file. Then, drag and drop the file you want to reference at the top of the document.

That's it. You now have Intellisense if a "vsdoc" of your library is available. I'll even throw you something more for you to enjoy. When you declare an event handler for a jQuery element and that you want to access the "event" element. That event is of type jQuery.Event. Just add "/// &lt;param name=variableName type=jQuery.Event /&gt;" right after the declaration and it will enable the Intellisense on that variable.

Enjoy!

