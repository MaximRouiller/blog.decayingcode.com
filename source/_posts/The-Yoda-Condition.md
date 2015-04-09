---
title: "The Yoda Condition"
date: 2014-11-12 13:51:59
tags: [opinion]
---

So this will be a short post. I would like to introduce a word in my vocabulary and yours too if it didn't already exist.

First I would like to credit [Nathan Smith](https://www.twitter.com/NathanSmith) for teaching me that word this morning. First, the tweet:
> Chuckling at "disallowYodaConditions" in JSCS… [https://t.co/unhgFdMCrh](https://t.co/unhgFdMCrh) — Awesome way of describing it. [pic.twitter.com/KDPxpdB3UE](http://t.co/KDPxpdB3UE)
> — Nathan Smith (@nathansmith) [November 12, 2014](https://twitter.com/nathansmith/status/532336447991586816)

<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

So... this made me chuckle.

### What is the Yoda Condition?

[The Yoda Condition](http://en.wikipedia.org/wiki/Yoda_conditions) can be summarized into "inverting the parameters compared in a conditional". 

Let's say I have this code:

```cs
string sky = "blue";
if(sky == "blue") 
{
    // do something
}
```

It can be read easily as "If the sky is blue". Now let's put some Yoda into it!

Our code becomes :
```cs 
string sky = "blue";
if("blue" == sky) 
{
    // do something
}
```

Now our code read as "If blue is the sky". And that's why we call it Yoda condition.

### Why would I do that?

First, if you're missing an "=" in your code, it will fail at compile time since you can't assign a variable to a literal string. It can also avoid certain null reference error.

### What's the cost of doing this then?

Beside getting on the nerves of all the programmers in your team? You reduce the readability of your code by a huge factor.

Each developer on your team will hit a snag on every if since they will have to learn how to speak "Yoda" with your code.

### So what should I do?

Avoid it. At all cost. Readability is the most important thing in your code. To be honest, you're not going to be the only guy/girl maintaining that app for years to come. Make it easy for the maintainer and remove that Yoda talk. 

The problem this kind of code solve isn't worth the readability you are losing.