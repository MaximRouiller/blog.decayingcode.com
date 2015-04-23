---
title: "The cost of Bad Code"
date: 2009-06-28 23:40:00
tags: []
---

Every developer writes code. Every developer works or has worked on a Brownfield project. Working on a Brownfield project often makes developer complain about the code being poorly written and hard to maintain. That surely sounds familiar, right?

This is basically a pledge to good code. Bad code makes things worse and cost business money.

#### How much are we talking about?

There is no scientific study about this. Primarily because most projects are private and won't allow studies and there is still no clear metric that represent clean code. Mostly, metrics can't represent bad code. So how much money can be saved? Well... bad code hinder maintenance, comprehension and scare programmers of changing a class that was working well before. I don't think we can calculate now, but I think that Cyclomatic Complexity, LOC per functions and Code Coverage represent a big indicator of code that are hard to understand and difficult to make changes.

Code that have high cyclomatic complexity and huge LOC per functions scares programmers into making changes. Why? Because we all know that if we change something inside one of those method, the ripples of change will make something else break. This fear can be neutralized by high code coverage of those big methods and/or by splitting them up.

Time for totally unscientific numbers. I think that complex code will require more double the time to make modifications to. Why? Well... let's say that the developer will have to spend a considerate amount of times in the debugger instead of running tests. Tests for a (big) module should take less than 10-15 seconds to run (including the test runner initialization). Debugging the same module to verify a behaviour will take normally a minute or two. Rinse and repeat at least a dozen times and you find yourself at 1 minutes for running the tests and 12 minutes for debugging an application. This is just the beginning. If there is no tests, a huge and complex method will take literally take at least 10 minutes to understand (depend of context). A test "infected" code base will allow for quick failure verification without having to spend hours in the debugger. Calculate as much as you want but... as [Robert C. Martin said](http://www.artima.com/weblogs/viewpost.jsp?thread=51769):

> The only way to go fast is to go well.

So are you saving time in your company or are you costing your company money? I think we can all earn something from writing clean code. Companies will save on maintenance cost, programmer will improve their craft and become better programmer that are proud of what they do.

