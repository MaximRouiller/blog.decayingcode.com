---
title: "ECMAScript 6 will be finalized this year. What should we expect now and next year?"
date: 2013-09-06 06:08:54
tags: [javascript]
---

# What is it?

ECMAScript 6 is the the new version of JavaScript. JavaScript is only an implementation to ECMAScript (currently ES5). ES6 will be the new standard for JavaScript for the browsers that support them. It will be finalized this year but browsers are already implementing some of the features that were proposed.

## What’s new?

The work on ECMAScript 6 has been going for a while now and a lot of things are coming our way.

The first thing to remember is that ES6 will be based upon ES5 with [Strict Mode](http://msdn.microsoft.com/en-us/library/ie/br230269(v=vs.85).aspx) enabled. This means using the more beautiful parts of ES5 and dropping a few of the bad parts. E.g.:&nbsp; it’s not possible to assign a value to a variable that hasn’t been declared, it’s not possible to write to a read-only property, it’s not possible to declare duplicate properties, etc. 

Among the most awaited features are [Classes](http://wiki.ecmascript.org/doku.php?id=strawman:maximally_minimal_classes) and [Typed Objects](http://wiki.ecmascript.org/doku.php?id=harmony:typed_objects). This will bring ES6 one step closer to maturity as a language. Having constructors and inheritance will definitely change the way code is done today. The prototyping paradigm can bring us a long way but having support for real types will remove the headache of designing useful code.

If you are doing UI on a daily basis, one of the most exciting feature for you will be the [observable](http://wiki.ecmascript.org/doku.php?id=harmony:observe) proposal. It brings something like [KnockoutJS observable](http://knockoutjs.com/documentation/observables.html) to the core of JavaScript. This might not impact your work immediately but as more frameworks make use of this feature, your code will end-up being more simple to handle. 

## What are the potential impacts?

If we are talking desktop browsing, I don’t expect much to change right away until the three major browsers has implemented ES6 properly. I don’t expect everyone to upgrade their code but we can expect framework developers to adapt to ES6 very rapidly. ES6-enabled framework should allow us to use fewer lines of codes for the same operations as well as richer features. 

However, mobile browsers evolve faster and more in a silo than other browsers. Developing an HTML application for WP8, Android or iOS will put you in the company’s silo and will allow you to target a very specific range of browsers.

One the major impact to be expected however would be on the end of those using NodeJS. Since JavaScript is running 100% on the server it’s easier for maintenance and updates to know what is supported and what isn’t. 

## When will it be available?

It’s currently being implemented by most major browsers (Internet Explorer, Firefox and Chrome). Firefox 25 was, at the time of this writing, the one the most ahead in term of implementation. As the specification is getting closer to being approved (end of this year), we should see Firefox and Chrome implement most of those features for 2014 and a bit later for Internet Explorer.

If you don’t want to search forever on which supports what, you can take a look at the [ES6 Compatibility Table](http://kangax.github.io/es5-compat-table/es6/) which is updated on a frequent basis. 

## Source

For the full list of proposal items that are tentatively accepted, see the [ECMAScript.org website](http://wiki.ecmascript.org/doku.php?id=harmony:proposals).