---
title: "Browser Detection is bad. Feature Detection is better."
date: 2012-02-17 12:44:00
tags: [javascript]
---

## Context (Or a little bit of history)

Those who did websites in the beginning of the decade had to work with IE6, Netscape Communicator/Navigator and Opera. Some more browser introduced themselves along the years but most supported different things.

Netscape supported the blink tag (oh how I hate thee). Internet Explorer supported ActiveX and Opera was not popular enough to think about it.

We were in an era where people thought of the web as the blue "E" icon. People were not aware of browsers that much and those who had Netscape probably had it from an AOL promotion.

Since we wanted to display the same thing to every users as much as possible, people either started detecting browsers to clearly say on which browser their website is supported or they just didn&rsquo;t do any checking at all!

That's the main reason some internal web applications still display "Works better with IE6".

## Browser Detection

Browsers back then differed a lot from each other. So we had to know if we could use ActiveX or whether we wanted to piss off our users with a blink tag.

The rendering of all this of course was different per browsers.

Here is [the script](http://www.quirksmode.org/js/detect.html) to detect which browsers you are running. I will not copy it here (too large) but you can see that it grows.

Once people knew which browser was running, they knew which feature was supported! So everything was perfect, right?

Not exactly. What if the feature was deprecated in the next version? What if the browser had that functionality disabled by an option? What if a new browser that your script don&rsquo;t know come in? Why couldn&rsquo;t he use that feature if he supported it?

This caused a lot of problem. Mostly because detecting a browser didn&rsquo;t guarantee you anything beside that the user was sending you a UserAgent string that pleased you.

The browser detection was broken from the start and something had to be done to detect whether something was going to work or not. Irrespectively of the browser.

## Feature Detection

Then came feature detection. The first real push for it was by Mr. John Resig himself when he posted ["Future-Proofing JavaScript Libraries"](http://ejohn.org/blog/future-proofing-javascript-libraries/). Today, jQuery allow developers to access a ton of features that would have taken individual developers months to develop and maintain. jQuery allows you to implement binding of events without knowing whether you should use [attachEvent](http://msdn.microsoft.com/en-us/library/ms536343(v=vs.85).aspx) or [addEventListener](https://developer.mozilla.org/en/DOM/element.addEventListener).

If you have the time, go see the development version of jQuery. It&rsquo;s commented and will allow you to find their hacks to offer everyone the same functionalities. This allows you to manipulate the browser without having to know which one it is. It allows you to do Ajax in [IE6 with ActiveX or with IE7](http://msdn.microsoft.com/en-us/library/ms537505(v=vs.85).aspx#_id) (or higher) with the proper object without knowing if your browser supports ActiveX.

Today, assuming everyone use a frameworks which smooth the basic differences between the browsers, the only thing you might be checking nowadays is HTML5 feature compatibility.

## Introducing Modernizr

[Modernizr](http://www.modernizr.com/download/) is a library which does Feature detection and that can be tailor cut to your needs. In its complete version, it allows you to detect easily the following features (not a complete list):

*   SVG/Canvas support
*   Web sockets
*   Web SQL Database
*   Web workers
*   HTML5 Audio/video
*   Hash Change event
*   WebGL
*   Geolocation
*   Much more&hellip;

Modernizr once referenced in your webpage allow you to tailor your own code to know if you should gracefully degrade some feature or not.

Here is an example on how you could use Modernizr when implementing geolocation in your application:

```js
Modernizr.load({
  test: Modernizr.geolocation,
  yep : 'geolocation.js',
  nope: 'no-geolocation.js'
});
```

This simple code allows you to load a different JavaScript based upon the feature of a browser.

Modernizr offer support for IE6+, FireFox 3.5+, Opera 9.6+, Safari 2+, Chrome, iOS, Android, etc.

## Conclusion

With the current quantity of browsers that supports different functionalities at various levels, we should not be implementing for specific browsers but rather for specific features. We would want those feature implemented in all browsers but with the fragmentation of the browser market, it&rsquo;s just not going to happen soon.

So please, no more "This site works better with [INSERT BROWSER]". Target a feature, test for it, and offer different (or no) implementation if the feature is not available. This will make your code cleaner and more efficient.

So stop the Browser detection madness and embrace the feature detection.

It might sound counter intuitive to some but it makes sense. So be ready for the future.

If you are using HTML5, use feature detection rather than browser detection.