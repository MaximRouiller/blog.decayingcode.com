---
title: "Anti-Pattern: The god object"
date: 2009-02-26 21:32:00
tags: []
---

Because it's easier to recognize evil if you have a mug shot, here's one simple for all of you. The god object is a class that knows too much. It's a severe violation of the [Single Responsibility Principle](http://en.wikipedia.org/wiki/Single_responsibility_principle) and probably a lot of the other principle of SOLID depending of the implementation.

A basic principle in programming is to divide a problem in subroutines to make the problem easier to solve. It's also know as "divide and conquer". The object becomes so aware of everything or all the objects become so dependant of the god object that when there is a change or a bug to fix, it become a real nightmare to implement.

I tend to call those objects "Death Stars". ![Death Star](http://lh4.ggpht.com/_S0YTV7NEdrk/SadQ2aX_LhI/AAAAAAAAABI/9hQAQLJ6GLw/Death_Star%5B9%5D.png?imgmax=800 "Death Star") Why? Because just like the death star, if someone get to mess up with the core, it will explode. In fact, any modification to this god object will cause ripple of changes everywhere inside the software and will end in lots of bugs.

You can easily recognize of these object by the fear any developers have when getting near it.

So how to solve it? By refactoring of course! The goal is to separate in as much subroutine as possible. Once this is done, you move those subroutines to different classes. Of course, trying to follow the SOLID principles will definitely help.

Resolving a god object is of course really different depending on how omnipotent your god object is. But one thing for sure, you have to separate the "powers" (read: responsibility) and make sure that Single Responsibility Principle is applied.
