---
title: "Adding color to your Javascript charts with Dimple and d3js (Part 2)"
date: 2014-08-08 20:43:17
tags: [chart,tool,javascript]
---

![](/posts/files/b8f28463-d5c3-42b1-bf3e-fb4dbf6fbe68.png)

So we started by doing some graphs from basic data. But having all the colors the same or maybe even showing bars is not enough.

Here are a few other tricks to make the graph a little bit nicer. Mind you, there is nothing revolutionary here… it’s all in the documentation. The point of this blog post is only to show you how easy it is to customize the look of your charts.

First thing first, here are [the sources](https://github.com/MaximRouiller/charting-blog-posts) we are working with.

### Showing lines instead of bars

Ahhh that is quite easy.

It’s actually as simple as changing the **addSeries** function paramter

Here’s what the current code look like now:
```js
var post2 = function() {
    // blog post #2 chart
    var svg = dimple.newSvg("#lineGraph", 800, 600);
    var chart = new dimple.chart(svg, csv);
    chart.addCategoryAxis("x", "Country");
    chart.addMeasureAxis("y", "Total");
    chart.addSeries(null, dimple.plot.line);
    chart.draw();
};
post2();
```

And the graph looks like this:

[![image](/posts/files/749cd9e7-88d2-40d9-adbb-ce7dd4998ec8.png "image")](/posts/files/66e483b5-c81d-47e4-b25f-ded0228487fd.png)

Simple enough?

Of course, this isn’t the type of data for lines so let’s go back to our first graph with bars and try to add colors.

### Adding a color per country

So adding a color per country is about defining the series properly. In this case… on “Country”.

Changing the code isn’t too hard:
```js
var post1 = function() {
    var svg = dimple.newSvg("#graphDestination", 800, 600);
    var chart = new dimple.chart(svg, csv);
    chart.addCategoryAxis("x", "Country");
    chart.addMeasureAxis("y", "Total", "Gold");
    chart.addSeries("Country", dimple.plot.bar);
    chart.draw();
};
post1();
```

And here is how it looks like now!

[![image](/posts/files/f4cf6eb8-ddfe-4468-a45e-073e7a93d8a9.png "image")](/posts/files/9a62a216-d11d-489f-9102-2154fcc660df.png)

Much prettier!!

Next blog post, what about adding some legends? Special requests?