---
title: "Part 1 - Introduction to Moq"
date: 2009-02-04 17:21:00
tags: []
---

See also: &Acirc;&nbsp;[Part 2](/2009/02/part-2-basic-of-mocking-with-moq.html)&Acirc;&nbsp;-&Acirc;&nbsp;[Part 3](/2009/02/part-3-advanced-mocking-functionalities.html)

This is the first post of a serie on mocking with [Moq](http://code.google.com/p/moq/ "Moq"). I'll be giving a conference a [.NET Montreal Community](http://www.dotnetmontreal.com/ ".NET Montreal Community") on February 25th and I though there it would be good reference to anyone attending the [@Lunch event](http://www.dotnetmontreal.com/dnn/Accueil/tabid/36/ModuleID/398/ItemID/29/mctl/EventDetails/language/en-CA/Default.aspx?selecteddate=25/02/2009).

##### What is Moq?

Moq is a mocking framework like RhynoMock and TypeMock jointly developed by Clarius, Manas and InSTEDD. It heavily use Lambda to create expectations and returning results. It's been highly criticized as not making any distinctions between Mocks and Stubs.

What is import to remember, is that unless you are philosophically attached to your testing style... most developer [don't make any different between them](http://www.clariusconsulting.net/blogs/kzu/archive/2007/12/27/48594.aspx) and rather do behaviour testing.

Moq easily allows you to change it's behaviour from "Strict" to "Loose" (Loose being the default). Strict behaviour won't allow any calls on the mocked object unless it's been previously marked as expected. Loose will allow all calls and return a default value for it.

There is a lot off more advanced behaviours that can be configured and used.

**Why another mocking framework?**

[Daniel Cazzulino](http://www.clariusconsulting.net/blogs/kzu/ "Daniel Cazzulino") (A.k.a Kzu) blogged a lot about Moq and even compared [why he helped in creating Moq](http://www.clariusconsulting.net/blogs/kzu/archive/2007/12/18/LinqtoMockMoqisborn.aspx). Moq was created to ease the learning curve of learning a mocking framework while blurring the distinctions between [mocks and stubs](http://www.clariusconsulting.net/blogs/kzu/archive/2008/07/05/77747.aspx).

Moq allow you to quickly get into mocking (a good thing) while allowing more complex scenarios by more purist mockists. It's the perfect mocking framework if you have never touched a mocking framework and is your first experience.

##### Where do I download it?

You can download Moq directly [here](http://code.google.com/p/moq/downloads/list). At the moment of writing this post, Moq was at version 2.6.1014 and Moq 3.0 was available as a beta.

##### How to install it?

Once Moq is downloaded and extracted from it's zip file, you can easily add the single DLL (Moq.dll) inside your project install it inside the GAC if you are going to use it on many projects.

##### Stay up to date

After this brief introduction, I'll show more advanced feature of Moq with code sample on how to use them and why to use them.