---
title: "Writing cleaner JavaScript code with gulp and eslint"
date: 2016-05-06 00:00:00
tags: [javascript]
---

With the new ASP.NET Core 1.0 RC2 right around the corner and it's deep integration with the node.js workflow, I thought about putting out some examples of what I use for my own workflow.

In this scenario, we're going to see how we can improve the JavaScript code that we are writing.

### Gulp

This example uses gulp. 

I'm not saying that gulp is the best tool for the job. I just find that gulps work really well for our team and you guys should seriously consider it.

### Base file

Let's get things started. We'll start off the base template that is shipped with the RC1 template. 

The first thing we are going to do is check what is being done and what is missing.

    /// <binding Clean='clean' />
    "use strict";

    var gulp = require("gulp"),
        rimraf = require("rimraf"),
        concat = require("gulp-concat"),
        cssmin = require("gulp-cssmin"),
        uglify = require("gulp-uglify");

    var paths = {
        webroot: "./wwwroot/"
    };

    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";

    gulp.task("clean:js", function (cb) {
        rimraf(paths.concatJsDest, cb);
    });

    gulp.task("clean:css", function (cb) {
        rimraf(paths.concatCssDest, cb);
    });

    gulp.task("clean", ["clean:js", "clean:css"]);

    gulp.task("min:js", function () {
        return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
            .pipe(concat(paths.concatJsDest))
            .pipe(uglify())
            .pipe(gulp.dest("."));
    });

    gulp.task("min:css", function () {
        return gulp.src([paths.css, "!" + paths.minCss])
            .pipe(concat(paths.concatCssDest))
            .pipe(cssmin())
            .pipe(gulp.dest("."));
    });

    gulp.task("min", ["min:js", "min:css"]);


As you can see, we basically have 4 tasks and 2 aggregate tasks.

* Clean JavaScripts files
* Clean CSS files
* Minimize Javascript files
* Minimize CSS files

The aggregate tasks are basically just to do all the cleaning or the minifying at the same time. 

### Getting more out of it

Well, that brings us to feature equality with what was available with MVC 5 with the Javascript and CSS minifying. However, why not go a step further?

### Linting our Javascript

One of the most common thing we need to do is make sure we do not write horrible code. Linting is a code analysis technique that detects early problems or stylistic issues.

How do we get this working with gulp?

First, we install `gulp-eslint` with `npm install gulp-eslint --save-dev` run into the web application project folder. This will install the required dependencies and we can start writing some code.

First, let's start by getting the dependency:

    var eslint = require('gulp-eslint');

And into your default ASP.NET Core 1.0 project, open up site.js and copy the following code:

    function something() {
    }

    var test = new something();

Let's run the `min:js` task with gulp like this: `gulp min:js`. This will show that our file is minimized but... there's something wrong with the style of this code. The `something` function should be Pascal cased and we want this to be reflected in our code.

Let's integrate the linter in our pipeline.

First let's create our linting task:

    gulp.task("lint", function() {
        return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
            .pipe(eslint({
                rules : {
                    'new-cap': 1 // function need to begin with a capital letter when newed up
                }
            }))
            .pipe(eslint.format())
            .pipe(eslint.failAfterError());
    });

Then, we need to integrate it in our minify task.

    gulp.task("min:js" , ["lint"], function () { ... });

Then we can either run `gulp lint` or `gulp min` and see the result.

> C:\_Prototypes\WebApplication1\src\WebApplication1\wwwroot\js\site.js
> 6:16  warning  A constructor name should not start with a lowercase letter  new-cap

And that's it! You can pretty much build your own configuration from the [available ruleset](http://eslint.org/docs/rules/) and have clean javascript part of your build flow!

### Many more plugins available

More gulp plugins are available on [the registry](http://gulpjs.com/plugins/). Whether you want to lint, transpile javascript (TypeScript, CoffeeScript), compile CSS (Less, SASS), minify images... everything can be included in the pipeline. 

Look up the registry and start hacking away!
