---
title: "Cross Platform PDF generation with ASP.NET Core and PhantomJS"
date: 2016-07-13 10:00
tags: [asp.net core, tools]
---

One of the easiest way I found to statically generate reports from .NET Core is to simply not to use ASP.NET Core for rendering PDF.

At the current state of affair, most libraries aren't up to date and may take a bit of time before they are ready to go.

So how do you generate PDF you say? By rendering HTML.

### What to render with ASP.NET Core

### How does it work with PhantomJS

By using scripts like [rasterize.js](https://github.com/ariya/phantomjs/blob/master/examples/rasterize.js) to interface with [phantomjs][phantomjs], you can easily create PDF files with ease.

### Cross-Platform concerns

TODO: what is phantoms js and why it works

[phantomjs]: http://phantomjs.org/
