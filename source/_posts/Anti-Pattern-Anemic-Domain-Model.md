---
title: "Anti-Pattern: Anemic Domain Model"
date: 2009-02-28 17:51:00
tags: []
---

Here is an anti-pattern [Martin Fowler](http://www.martinfowler.com/bliki/) will agree with. In fact, it's Martin Fowler that [first described this anti-pattern](http://www.martinfowler.com/bliki/AnemicDomainModel.html) in November 2003\. Like Fowler said, it looks like a model, it smells like a model but there is no behaviour inside.

> The basic symptom of an Anemic Domain Model is that at first blush it looks like the real thing. There are objects, many named after the nouns in the domain space, and these objects are connected with the rich relationships and structure that true domain models have. The catch comes when you look at the behavior, and you realize that there is hardly any behavior on these objects, making them little more than bags of getters and setters.

The problem with the anemic domain model is that all the logic is not with the associated object. It's located in the objects that use them. See the problem? So unless you are using the objects that have the behaviours, having the anemic domain model won't bring you any good. In fact, they just getters and setters with barely enough behaviour to call them objects.

Of course, you gain a good separation of concerns and a gain in "flexibility" of behaviours if ever needed. You also gain the ability to generate those domain models from a modeling tools without having to break a sweat. If there is so many benefits, where's the catch?

3 times nothing! You just need to separate the business logic multiple time so that every part of the business get their own. Objects can't self validate since the validation logic is located outside of the object. Everyone need a reference to that specific model DLLs and any shared entities which increase the coupling of the classes. It also increase the code duplication since many part of the business will essentially reuse many parts that other part of the business need. Don't forget the maintenance! Since the business logic is spread across the business, all the common business logic will need to be updated all at once and validated against their respective service and validation. And that, is if this part of the business want to update. It's like dealing with many mini-companies within the same companies.

Got enough? So put some business logic inside your domain model and make it easy to understand. If a certain part of the business need so "special" behaviour, it will have to be incorporated inside the main domain model.

But what if you have to maintain an anemic domain model and you want to fix this anti-pattern? Of course you can always rewrite the software but it's an expensive solution. The solution is that every time a new requirements arrive, put it inside the domain model and **DO NOT** put it inside the many service class. This is what Greg Young described as "making bubbles". By making bubbles of great code that is going to be easy to maintain/reuse, and by maintaining the current system, you will finish by replacing everything.

Anything worth doing is worth doing well.