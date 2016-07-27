---
title: "Writing testable JavaScript: How AMD changed the way I see code"
date: 2014-05-02 13:30:00
tags: [javascript,unit test]
---

Well to start, what is AMD? This acronym means Asynchronous Module Definition.

In the old days of JavaScript development, we defined functions in JS files (or maybe you just wrote them inline in the HTML) and included them in our master page. Those modules got defined in the order we defined them whether we used them or not. Some errors could be possible by defining a JS file before another it depended upon. This would cause an errors when the page loaded without a clear answer of what was the problem. We would reorder our file declaration, hit save and reload.

Did you do that? I sure still do it (kinda) today. Especially for everything that is related to jQuery and its plugins. You make sure to define jQuery before the other modules and it’s almost guaranteed to always work. But here’s where it gets hairy.

When you write application code, there is many ways to do it. 10-15 years ago, people used to put all their code into one file called “app.js” or something along those lines. When you are downloading file at the blazing speed of 5 kilobytes per seconds, it’s important to minimize the amount of requests on your website and every request chewed precious seconds for your user.

### Things have changed.

With tools like the built-in .NET Bundler and Minifier or packages like [BundleTransformer](http://bundletransformer.codeplex.com/), it’s now easy to package up your applications in one big file and have everything compressed to save on space. So the excuse to not separate your application in multiple files has been rendered moot in the last 5 years.

So we did. We split all our code in plugins, accessing the DOM with jQuery like never before. Splitting every component in a global namespace “window.MyApp.MyPage.myProperty” but it didn’t feel clean. Having to handle namespaces is boring very fast. You write a lot of boilerplate code like this:

```js
window.myNamespace = window.mynamespace || {};
window.myNamespace.anotherOne = window.myNamespace.anotherOne || {};
// ... etc... etc...
```

And you had to write this at the beginning of every file to ensure that in whichever order things were loaded, you could actually use them. There had to be another way.

### Introducing requirejs

[requirejs](http://requirejs.org/) is a tool that allows you to work with modules (one per file) instead of blocks of code. When writing a module, you can request dependencies.

```js
define(["widget/myWidget", "myplugin"],
    function(myWidget, myPlugin) {
        //todo: write awesome code
        return "...";
    });
```

So you tell requirejs that you want some plugin and requirejs will ensure to download the required file, ensure that this file had their dependencies resolved and so on. All of this being auto-magically downloaded for you.

Another free bonus is we return a static contract. An object which will represent your whole module. This is way better than global function. First, you keep your private to yourself and expose what you want to be exposed. This allows for a behaviour similar to a class.

### However, there’s a problem. Do you see it?

It downloads file individually, which is fine in development but becomes a problem in production even with our super fast connections. Even if connections have goten faster, latency (the milliseconds taken for the server to send you the first byte) can kill your performance if you request 20 files (20 files at 200ms is 4 seconds of wait). The classic .NET bundler is not the best tool with those kind of app since they are not contextually aware of the structure of your app.

It works best with something like [NodeJS](http://nodejs.org/) where access to files are local and not remote.

### How can we fix this?

With NodeJS and r.js. Why? Because NodeJS allows you to run JavaScript outside of a browser and access your file system to retrieve all the required files. The RequireJS team wrote an optimizer for NodeJS that will compile all the modules you write into a single file.

Basically, you just show him what you want optimized, run the command line and let it go.
```bash
node r.js -o myapp.build-configuration.js
```

I’m not going to cover it all. [Read the requirejs documentation](http://requirejs.org/docs/optimization.html#wholeproject) for more info.

### What are the advantages in using AMD/requirejs?

First, a module is highly testable. If you want to finally cover your client-side code with unit tests… it’s one of the more efficient way to do it. You define a module, you test its public components.

Second, splitting files for each part of your app instead of having a global file will definitely give a cleaner vision of what is going on in your application. The single application.js file with 20,000 lines of code is not funny anymore. It was fun when we saw an IDE crash from opening the file the first few times but we’re getting serious now. One file per page, widget, dialog and what not is the norm in most serious JavaScript application.

Third, having your dependencies resolved like your favorite package manager (npm, NuGet, etc.) is what we expect out of our tools. Tell the tool what you want, let the tool find your dependency. No more playing with global variable and playing the classic JavaScript Russian Roulette. Changing global state and hoping my code doesn’t break when I don’t click on the right buttons in the right sequence is not the way to go. Know what you are modifying at all time by looking at the requested dependencies.

Finally, having the peace of mind of knowing that your helped make Web Development a little bit better.

So I know that I haven’t covered everything but the point was to help convince you that asynchronous modules are the way to go. Have I convinced you?
