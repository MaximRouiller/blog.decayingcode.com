---
title: "Improving code quality - 2 ways to go"
date: 2009-06-22 01:05:00
tags: []
---

I've been thinking about this for at least a week or two. In fact, it's been since I started (and finished) reading the book ["Clean Code" by Robert C. Martin](http://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882). There is probably only two way to go.

#### Fix the bad code

This method is called refactoring and "cleaning" the code. This of course, you can't truly know what code is bad without having a Static Analyser tool or programmers working on the code. The tool will allow you to spot piece of code that could bring bugs and/or be hard to work with. The problem, refactoring code or cleaning up code is really _expensive_ on a business perspective. The trick is to fix it as you interact with the code. It is probably impossible to request time from your company to fix code that **could** cause bugs. If you ask your company to fix the code, you will probably receive this answer: "Why did you write it badly in the first place?". Which brings us to the other way to improve the code quality.

#### Don't write it

If you don't write the bad code in the first place, you won't have to fix it! That sounds simple to an experienced programmer that improved his craft with years but rookies will definitely leave bad code behind. Eventually, you will have to encounter this bad code. So how do you avoid the big refactoring of mistakes (not just rookies)? I believe that training might be a way to go. When I only had 1 year of experience in software development, I was writing WAY too many bad code. I still do. Not that I don't see it go through. Sometimes, things must be rushed, I don't understand fully the problem and some small abstraction mistakes gets in. I write way less bad code then when I started. However, this bad code is not just magically disappearing. It's stays there.

#### What about training?

I think that training and/or mentoring might be the way to go. Mentoring might be hard to sell but training is definitely not that hard to sell. Most employees got an amount of money related to their name within a company that represent training expenses that can be spent on them. What I particularly recommend is some courses in Object-Oriented design or Advanced Object-Oriented Design. Hell, you might even consider an xDD course (and by xDD... I mean TDD, BDD, DDD, RDD, etc.). Any of those courses will improve your skill and bring you closer to identifying clean code from bad code. Other training that will form you specific framework (like ASP.NET MVC or Entity Framework) will only show you how to get things done with those framework. The latter can be learned on your own or through a good&nbsp; book.

So? What do you all thinks? Do you rather have a framework course or a "Clean Code" course?