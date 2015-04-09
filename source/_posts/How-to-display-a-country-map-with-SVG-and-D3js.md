---
title: "How to display a country map with SVG and D3js"
date: 2014-08-26 22:49:52
tags: [javascript,chart]
---

I’ve been babbling recently with charts and most of them was with DimpleJS.

However, what is beside DimpleJS is d3.js which is an amazing tools for drawing anything in SVG.

So to babble some more, I’ve decide to do something simple. Draw Canada.

### The Data

I’ve taken the data from [this repository](https://github.com/johan/world.geo.json) that contains every line that forms our Maple Syrup Country. Ours is called “CAN.geo.json”. This file is called a [Geo-Json](http://geojson.org) file and allows you to easily parse geolocation data without a hitch. 

### The Code

```js
var svg = d3.select("#chartContainer")
    .append("svg")
    .attr("style", "solid 1px black")
    .attr("width", "100%")
    .attr("height", "350px");

var projection = d3.geo.mercator().center([45, 55]);
var path = d3.geo.path().projection(projection);

var g = svg.append("g");
d3.json("/data/CAN.geo.json", function (error, json) {
    g.selectAll("path")
           .data(json.features)
           .enter()
           .append("path")
           .attr("d", path)
           .style("fill", "red");
});
```

### The Result

<div id="chartContainer"></div><script type="text/javascript" src="/scripts/d3.v3.min.js"></script><script type="text/javascript">
            $(function() {
                var svg = d3.select("#chartContainer")
                    .append("svg")
                    .attr("style", "solid 1px black")
                    .attr("width", "100%")
                    .attr("height", "350px");

                var projection = d3.geo.mercator().center([45, 55]);
                var path = d3.geo.path().projection(projection);

                var g = svg.append("g");
                d3.json("/data/CAN.geo.json", function (error, json) {
                    g.selectAll("path")
                           .data(json.features)
                           .enter()
                           .append("path")
                           .attr("d", path)
                           .style("fill", "red");
                });
            });
        </script>

### Conclusion

Of course this is not something very amazing. It’s only a shape. This could be the building block necessary to create the next eCommerce world-wide sales revenue report. 

Who knows… it’s just an idea. 