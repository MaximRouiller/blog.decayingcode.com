---
title: "Debugging the web with Chrome"
date: 2016-07-07 10:44:00
tags: [javascript,tool,web]
---

There are so many ways to do web development today. But the most common scenario you are going to encounter, is debugging a web page.

Whether you are in jQuery or AngularJS, at some point you will need to display a variable's value or break somewhere that is hard to reach. The best tool for me to debug a web page is Google Chrome but some of those tricks might work in other browsers.

So let me show you my favorite, yet less known, tricks about debugging web pages.


### `console.table`

Have you ever had an array with lots of rows and start expanding the values looking for a specific object?

![Google Chrome Array](/posts/files/chrome-big-array.png)

Well there is a better way.

![Google Chrome console.table(...)](/posts/files/chrome-big-array-console-table.png)

If you only want certain properties you can even pass in the fields you want.

```javascript
console.table(array, ["name", "age"])
```

So stop expanding your objects one by one and use `console.table` to view them all at once.

### debugger

```javascript
function myFunction() {
  // do stuff
  debugger;
}
```

This one is easy. If the developer console is opened, your browser will break on the `debugger` line as soon as it reach it. Of course, be aware that you cannot disable this breakpoint.

### DOM elements in the console

Have you ever displayed a DOM element in the console?

![DOM element in console](/posts/files/dom-element.png)

But what if you want to see the actual JavaScript properties of the DOM element? `console.dir(...)` is your friend.

![console.dir](/posts/files/console-dir.png)

`console.dir` force the JavaScript representation of any object that you are trying to display.

### Profiling your code

Sometimes, code runs slowly. Profiling with Chrome is extremely easy. You click start, you run your piece of code and you press stop. Easy right?


But that will record anything that happens at that moment. What if the problematic code is located in a specific execution path in you want to just profile this part? I have the solution for you.

```javascript
function (){
  console.profile("Slow Code");
  slowFunction();
  console.profileEnd("Slow Code");
}
```
Running this type of code will give you this output in your `Profile` tabs in Chrome Developer tools.

![Google Chrome Profiling Session](/posts/files/chrome-profile.png)

### Debugging devices

You are probably developing on a desktop (or laptop) with a large display resolution. However, mobile is also an important focus for your organization when developing web apps.
How do you test multiple resolutions? Different media queries?

Most people resize their browsers. Let me show you a better way.

First activate the device toolbar by clicking here.

![Device Toolbar Button](/posts/files/device-toolbar.png)

You will then see this bar at the  top.

![Device Toolbar Expanded](/posts/files/device-toolbar-expanded.png).

Do you see the three dots on the right? Click on it.

![Device Toolbar Option](/posts/files/device-toolbar-options.png)

From there, you can activate media queries, rulers, media queries and really boost what you can do with Chrome.

Need to test specfici media queries? Yep. Need to test your website on a GPRS connection? 2 clicks away. Resize your viewport? Easy!

### Wrapping it up

Of course, there is so many more things that can be done with Chrome that I could talk about but I decided to focus on those that I used the most.

I will however leave some links that will allow you to explore more options!


### Links

* [Google Chrome Console API Reference](https://developer.chrome.com/devtools/docs/console-api)
* [Google Chrome Developer Tools Settings](https://developer.chrome.com/devtools/docs/settings)
* [JavaScript CPU Profiling](https://developer.chrome.com/devtools/docs/cpu-profiling)
* [Using the console](https://developer.chrome.com/devtools/docs/console)
