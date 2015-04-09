---
title: "Incompatibility between Nancy and Superscribe"
date: 2014-05-30 13:48:29
tags: [owin,superscribe,nancyfx]
---

So I’ve had the big idea of building an integration of [Nancy](http://nancyfx.org) and [Superscribe](http://superscribe.org) and try to show how to do it.

Sadly, this is not going to happen.

Nancy doesn’t treat routing as a first class citizen like Superscribe does and doesn’t allow interaction with routing middleware. Nancy has its own packaged routing and will not allow Superscribe to provide it the URL.

Nancy does work with Superscribe but you have to hard-code the URL inside the NancyModule. So if you upgrade your Superscribe graph URL, Nancy will not respond on the new URL without you changing the hardcoded string.

I haven’t found a solution yet but if you do, please let me know!