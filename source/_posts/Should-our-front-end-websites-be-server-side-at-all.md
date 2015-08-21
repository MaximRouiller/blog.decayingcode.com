---
title: "Should our front-end websites be server-side at all?"
date: 2015-08-20 15:00:00
tags: [opinion,architecture]
---

I've been toying around with projects like Jekyll, Hexo and even some hand-rolled software that will generate me HTML files based on data. The thought that crossed my mind was...

> Why do we need dynamically generated HTML again?

Let me take examples and build my case.

## Example 1: Blog

Of course the simpler examples like blogs could literally all be static. If you need comments, then you could go with a system like [Disqus](http://www.disqus.com). This is quite literally one of the only part of your system that is dynamic.

RSS feed? Generated from posts. Posts themselves? Could be automatically generated from a databases or Markdown files periodically. The resulting output can be hosted on a [Raspberry Pi](https://www.raspberrypi.org/documentation/remote-access/web-server/nginx.md) without any issues.

## Example 2: E-Commerce

This one is more of a problem. Here are the things that don't change a lot. Products. OK, they may change but do you need to have your site updated right this second? Can it wait a minute? Then all the "product pages" could literally be static pages.

Product reviews? They will need to be "approved" anyway before you want them live. Put them in a servier-side queue, and regenerate the product page with the updated review once it's done.

There's 3 things that I see that would require to be dynamic in this scenario.

Search, Checkout and Reviews. Search because as your products scales up, so does your data. Doing the search client side won't scale at any level. Checkout because we are now handling an actual order and it needs a server components. Reviews because we'll need to approve and publish them.

In this scenario, only the Search is the actual "Read" component that is now server side. Everything else? Pre-generated. Even if the search is bringing you the list of product dynamically, it can still end up on a static page.

All the other write components? Queued server side to be processed by the business itself with either Azure or an off-site component.

All the backend side of the business (managing products, availability, sales, whatnot, etc.) will need a management UI that will be 100% dynamic (read/write).

## Question

So... do we need dynamic front-end with the latest server framework? On the public facing too or just the backend?

If you want to discuss it, Tweet me at [@MaximRouiller](https://www.twitter.com/MaximRouiller).
