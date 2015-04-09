---
title: "SVG are now supported everywhere, or almost"
date: 2014-10-10 17:06:12
tags: [javascript,html5,opinion]
---

I remember that when I wanted to draw some graphs on a web page, I would normally have 2 solutions

Solution 1 was to have an IMG tag that linked to a server component that would render an image based on some data. Solution 2 was to do Adobe Flash or maybe even some Silverlight.

### Problem with Solution 1

The main problem is that it is not interactive. You have an image and there is no way to do drilldown or do anything with it. So unless your content was simple and didn't need any kind of interaction or simply was headed for printing... this solution just wouldn't do.

### Problem with Solution 2

While you now get all the interactivity and the beauty of a nice Flash animation and plugin... you lost the benefits of the first solution too. Can't print it if you need it and over that... it required a plugin.

For OSX back in 2009, [plugins were the leading cause of browser crash](http://www.macworld.com/article/1140897/keynote.html) and there is nothing that stops us from believing that similar things aren't true for other browsers.

The second problem is security. A plugin is just another attack vector on your browser and requiring a plugin to display nice graphs seem a bit extreme.

### The Solution

The solution is relatively simple. We need a system that allows us to draw lines, curves and what not based on coordinate that we provide it.

That system should of course support colors, font and all the basic HTML features that we know now (including events).

### Then came SVG

[SVG](http://en.wikipedia.org/wiki/Scalable_Vector_Graphics) has been the main specification to drawing anything vector related in a browser since 1999\. Even though the specification started at the same time than IE5, it wasn't supported in Internet Explorer until IE9 (12 years later).

The support for SVG is now in all major browsers from Internet Explorer to FireFox and even in your phone.

Chances are that every computer you are using today can render SVG inside your browser.

### So what?

SVG as a general rule is under used or thought of something only artists do or that it's too complicated to do.

My recommendation is to start cracking today on using libraries that leverage SVG. By leveraging them, you are setting yourself apart from others and can start offering real business value to your clients right now that others won't be able to.

SVG has been available on all browsers for a while now. It's time we start using it.

### Browsers that do not support SVG

*   Internet Explorer 8 and lower
*   Old Android device (2.3 and less), partial support for 3-4.3

### References, libraries and others

*   [Can I use SVG?](http://caniuse.com/#search=svg)
*   [D3.js](http://jgm.github.io/stmd/js/d3js.org)
*   [DimpleJS](http://jgm.github.io/stmd/js/dimplejs.org)
*   [ChartistJS](http://gionkunz.github.io/chartist-js/)
*   [Interactive map with D3.js](http://www.tnoda.com/blog/2013-12-07)