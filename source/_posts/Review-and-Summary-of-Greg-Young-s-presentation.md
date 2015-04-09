---
title: "Review and Summary of Greg Young's presentation"
date: 2009-02-17 20:00:00
tags: []
---

Greg had a presentation here in Montreal for the user group ".NET Montreal Community". I didn't know Greg before his presentation. He was talking about "Everything you wanted to know about architecture but were afraid to ask". I must say that this presentation was really revealing to some point. Greg really made a point that customers are the architect's priority and that the job of the architect is to bring IT and Business together.

His presentation was energetic, funny (man... you should see those slides!) and when you left... you learned something.

Here's what I remember 24 hours after the presentation.

#### Context is everything.

Pushing a prototype directly to production might be a good idea if it means the company's survival to get this product available. It all depend on... the context! If the application is a startup and no money is being made, the faster you have a product, the faster money starts to get in. In this scenario, you **have** to make some sacrifices. Instead of configuring a BizTalk installation, you might do a simple In-Code workflow that will help you get the first version out. It is not however a reason to keep the code crappy. This is a technical debt and will need to be paid back later. When the product/business starts making money, a proper BizTalk installation will have to be done and rewriting the workflow part will probably be time intensive.

#### Software is there to bring money or to save money

Which bring us to this. Money.

A software is not there to save reports to a database. It's there to save money by automating the reports creation so that someone doesn't have to spend 3 hours making a report (saving money here!).&Acirc;&nbsp; It's there to MAKE money in the case where you are actually building a product or offering a service to potential client. By keeping this in mind, we keep a better view of what the client really want.

#### When in a [BBoM](http://en.wikipedia.org/wiki/Big_ball_of_mud "Big Ball of Mud"), build bubbles

If you are in a situation where all the code is crap, start by building a bubble around what you are currently implementing so that other system can't break you. By bubble, we mean "clean code" that will do the work it is supposed to do, be testable, perform, etc..

Of course the bubble is going to be pierced and demolished with time but the longer people work on the system and the more bubble you build and in the end, you finish by having a nice encapsulated system with less mud. There will still be mud. It never will go totally away. But way less than if you would have thrown garbage code everywhere.

#### TTL (time to live) of a system is important

If the system is going to be rebuilt every 3 years, it's worthless to build it like it's going to last 25\. A lot of money can be saved when you don't have to make code that will last 25 years.

It doesn't mean that QoS and other non-functional requirements are not necessary. It is as necessary as anywhere. It just means that there won't be a 8 month planning to build the best architecture possible so that it can actually overcome **ANY** changes the business might have. By reducing the amount of architecture and choosing a simpler architecture that will be easier to maintain but less impervious to changes. Complexity to such a system will eventually increase up to a point where a rewrite is necessary. But here is the trick... a **planned rewrite**. Because of this, less time is spent on development and more time is spent on making money. Business loves that.

#### Architect must keep sight of the current context

Let's take an example. You have been hired by a small retail business to build an e-Commerce web site. The company is a start-up and living off debt. Their website is scheduled to be release in a month and they are expecting this money to payoff their debt and increase the sales.

Of course, you could start by building a site that will suite their needs and will start making them money. But you can also build the Next-Generation-E-Commerce-Application-That-Will-Blow-Competitors-Away but that will take 2 years to complete.

The goal is to keep sight on what the client need. The client want to make money. It's boring. It's more fun to build something that is incredibly powerful and that will be a real technical pleasure to build. If you chose #2, you just lost sight of your context.

The client would need money but you are lost on building "the next big thing".

#### Conclusion

Context is king for an architect. Craftsmanship must take the second place when making money (thus paying YOU) comes in the highest priority. It's not however a reason to slack off and write crap code. The better our code the easier to maintain and the easier to add new functionality it is. There is just times that prototypes are going to be shipped in production because it's essential to the business and that you are going to maintain it. The main goal is to be aware when we get a technical debt and to be able to reimburse it as soon as possible to keep the client happy.

The presentation Greg gave was very interesting and I really would love to see another presentation by Greg (DevTeach 2009? Montreal User Group?). If I got anything wrong in here, please tell me. I would love to discuss it with more people!

