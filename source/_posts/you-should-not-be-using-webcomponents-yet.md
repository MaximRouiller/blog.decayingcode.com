---
title: "You should not be using WebComponents yet"
date: 2015-06-17 00:00:00
tags: [javascript, html]
---

Have you read about [WebComponents]? It sounds like something that we all tried to achieve on the web since... well... a long time.

If you take a look at the [specification][WebComponentsSpec], it's hosted on the W3C website. It smell like a real specification. It looks like a real specification.

The only issue is that Web Components is really four specifications. Let's take a look at all four of them.

### Reviewing the specifications

#### HTML Templates

[Specification](http://www.w3.org/TR/2014/NOTE-html-templates-20140318/)

This specific specification is not part of the "Web components" section. It has been integrated in HTML5. Henceforth, this one is safe.

#### Custom Elements

[Specification](http://www.w3.org/TR/2014/WD-custom-elements-20141216/)

> This specification is for review and not for implementation!

Alright no let's not touch this yet.

#### Shadow DOM

[Specification](http://www.w3.org/TR/2014/WD-shadow-dom-20140617/)

> This specification is for review and not for implementation!

Wow. Okay so this is out of the window too.

#### HTML Imports

[Specification](http://www.w3.org/TR/2014/WD-html-imports-20140311/)

This one is still a working draft so it hasn't been retired or anything yet. Sounds good!

### Getting into more details

So open all of those specifications. Go ahead. I want you to read one section in particular and it's the author/editors section. What do we learn? That those specs were draft, edited and all done by the Google Chrome Team. Except maybe HTML Templates which has Tony Ross (previously PM on the Internet Explorer Team).

What about browser support?

Chrome has all the spec already implemented.

Firefox implemented it but put it behind a flag (about:config, search for properties `dom.webcomponents.enabled`)

Internet Explorer, they are all [Under Consideration](http://dev.modern.ie/platform/status/?filter=f3f0000bf&search=webcomponents)

### What that tells us

Google is pushing for a standard. Hard. They built the spec, pushing the spec also very hary since all of this is available in Chrome STABLE right now. No other vendors has contributed to the spec itself. [Polymer] is also a project that is built around WebComponents and it's built by... well the Chrome team.

That tells me that nobody right now should be implementing this in production. If you want to contribute to the spec, fine. But WebComponents are not to be used.

Otherwise, we're only getting in the same issue we were in 10-20 years ago with Internet Explorer and we know it's a painful path.

### What is wrong right now with WebComponents

First, it's not cross platform. We handled that in the past. That's not something to stop us.

Second, the current specification is being implemented in Chrome as if it was recommended by the W3C (it is not). Which may lead us to change in the specification which may render your current implementation completely inoperable.

Third, there's no guarantee that the current spec is going to even be accepted by the other browsers. If we get there and Chrome doesn't move, we're back to Internet Explorer 6 era but this time with Chrome.

### What should I do?

As for what "Production" is concerned, do not use WebComponents directly. Also, avoid Polymer as it's only a simple wrapper around WebComponents (even with the polyfills).

Use other framework that abstract away the WebComponents part. Frameworks like [X-Tag] or [Brick]. That way you can benefit from the feature without learning a specification that may be obsolete very quickly or not implemented at all.

[Polymer]: https://www.polymer-project.org/1.0/
[Brick]: https://mozbrick.github.io/
[X-Tag]: http://www.x-tags.org/
[WebComponentsSpec]: http://www.w3.org/standards/techs/components#w3c_all
[WebComponents]: http://webcomponents.org/
