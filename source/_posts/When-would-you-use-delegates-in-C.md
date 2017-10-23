---
title: "When would you use delegates in C#?"
date: 2009-02-17 23:17:00
tags: []
---

This is a valid [question](http://stackoverflow.com/questions/191153/when-would-you-use-delegates-in-c). Before C# 3.0, you could use delegates or declare full methods to bind to events. Now we can declare event directly through lambda. (See [this post](http://blog.decarufel.net/2009/02/10-reasons-why-net-sucks.html) on many different examples on how to bind event handlers).

[Jon Skeet](http://pobox.com/~skeet/csharp) answered me the following:

*   &nbsp;

    *   Event handlers (for GUI and more)*   Starting threads*   Callbacks (e.g. for async APIs)*   LINQ and similar (List.Find etc)*   Anywhere else where I want to effectively apply "template" code with some specialized logic inside (where the delegate provides the specialization)

Delegates is a keyword that can be used to declare inline methods. This inline code can be stored inside variables and then executed when necessary. This is exactly what happens when you are binding methods to events. You are storing the signature of the method inside a variable that will store multiple methods signature and call them when an event is happening.

Of course, it's limiting to think about delegates only as events. If we check the standard definition for the word [delegation](http://en.wikipedia.org/wiki/Delegation_(programming)):

> In its original usage, delegation refers to one object relying upon another to provide a specified set of functionalities. [...] Delegation is the simple yet powerful concept of handing a task over to another part of the program.

As I already demonstrated inside the [StreamProxy](/2009/01/creating-streamproxy-with.html) class, we can easily give another software the tools&nbsp; to answer it's solution. But sometimes, the call might not be necessary. Just like when you are sending a data repository to a service class to save a model, delegate is basically just sending any method that match the accepted signature&nbsp; instead of complete class.

One of the most recent use of Lambda's inside C# is inside Mocking tools. Moq use those to easily describe expectations, returned values, and so on. This allow Moq to be type safe instead of relying on Reflection and string comparison. This has brought us compile time check rather than runtime check.

There is a lot of use for delegates and they are being used more and more. Lots of languages support some form of delegation (.NET, C++, JAVA, and many more)

Hope delegates are not as foreign as they were 1 year ago.

