---
title: "Back to basics: Why should I use interfaces?"
date: 2010-02-04 12:30:00
tags: []
---

So I had this interesting discussion with a colleague about having a clean architecture for a small software he is doing. Since it's his first step among [SOLID](http://en.wikipedia.org/wiki/Solid_%28Object_Oriented_Design%29), I wanted to take it easy see how things were laid out.          Since the program was mostly already written, I immediately noticed the lack of pattern and the direct data access in the event of his WinForm application. The conversation went a bit like this:

> <span style="font-weight: bold;">Me</span>: What is this code with data access on the "OnClick" of your button?         <span style="font-weight: bold;">Him</span>: Well it's the information I need to execute this command.         <span style="font-weight: bold;">Me</span>: Do you know the Model-View-Presenter pattern? Because right now, you are mixing "Presentation", "Data Access" and "Business Logic"         <span style="font-weight: bold;">Him</span>: I've used it before but it's been a while. How do you implement it?

So after showing him the pattern and explaining the basic implementation (because there is a lot of different way to implement this pattern), he asked me the following question:

> "Of course, you don't need to use interface everywhere, right?"

But then I went on to explain testability and such but there is something different I wanted to bring to this small discussion and that I wanted to share a bit.          When my class have dependencies injected through the constructor, I have 2 choices. Either I depend upon the implementation or the abstraction (interface/abstract). What's the difference and why is it so important?

#### MyClass depending upon the abstraction of "MyClassDataAccess"

When your class depend upon the abstraction, it can take any class that implement that abstraction (be it abstract class or interface). The implementation can easily be replaced by something else and that is essential in unit testing your logic.

#### MyClass depending upon the implementation of "MyClassDataAccess"

When your class depend upon the implementation directly, the only that can be sent to this class is this specific implementation. Anything else must derive from this class. This implementation couple the Caller and the Callee really tightly.

#### Why is it important ?

When you have a class that access services, slow resource (database, disk, etc.) or even a class that you haven't coded yet... an interface should be used. Of course it's not a law. You apply interface/abstract class when you need to decouple an implementation of a system from another system.          That allows me to send mocked object and test my requirements/logic. This also bring another advantage that might not be evident at first. Customer changing his mind. When the customer change his mind and do not want to store information in an XML but instead want a database. Or when a customer say to not implement "this part of the system" because it will be available through a service. Etc Etc...          Using interfaces and abstract class is the oil that makes the engine of your software turn smoothly and allow you to replace parts by better/different parts without hell breaking loose because of tightly coupled implementation.
