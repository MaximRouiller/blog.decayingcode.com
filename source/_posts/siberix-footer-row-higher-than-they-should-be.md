---
title: "Siberix – Footer row higher than they should be"
date: 2011-11-23 12:56:39
tags: []
---

So I’ve had to build some reports with [Siberix](http://www.siberix.com/) in the last few days and I had some less than pleasing results. I had a grid that was overflowing vertically but for some reason, the last row on the page would take 2-3 times more space than necessary.

After printing the page and looking at the results many times… something clicked when I saw the footer row that I had added. The blank space that was left was exactly the same size as my footer.

So maybe it’s a bug, maybe it’s not but Siberix let’s itself some room on every page to render the footer “in case” that it’s the last page. No concept what so ever of where he’s at when rendering the PDF.

The solution? Make the footer outside the IGrid and it won’t glitch anymore. And yes, I tried to set the IFooter.Repeat to false.

I can understand that a product have bugs but the worst part is that it’s a paying tool and no community built around it like Telerik, ReSharper or other third party tools. My last try was with [Stackoverflow](http://www.stackoverflow.com) but didn’t have any luck either.

Siberix, if you want to increase your sales, make sure you have a community behind it. Just a friendly advice.
