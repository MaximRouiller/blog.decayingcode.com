---
title: "TypeScript or good old JavaScript?"
date: 2016-07-11 10:13:00
tags: [javascript, typescript]
---

Should I use TypeScript instead of pure JavaScript?

With Angular 2 right around the corner and [TypeScript 2.0 beta][1] just being released, a lot of questions are being asked. Should I keep on coding in JavaScript or should I give TypeScript a try?

If you aren't on TypeScript yet, it's basically a superset of JavaScript. All JavaScript is TypeScript but TypeScript isn't valid JavaScript. Follow me?

### Why TypeScript?

So what does TypeScript bring to the table?

* Static Typing (classes + interfaces)
* Funcitons
* Lambdas
* Namespaces
* Modules
* Enum
* etc.

This allow IDE to implement static typing, validation and better code management (namespaces, modules, etc.).

Of course, another valid option is to go ES6 and just [babel](https://babeljs.io/) your code. However, I like the idea of introducing a new language. It breaks the chains and expectations that are brought from JavaScript.

### Gulp Pipeline

With standard JavaScript development, I normally need to JSHint, concat, uglify and jasmine my code (translation: lint, bundle, minify and unit test).  In my standard gulp workflow, that means I have to use `jshint`, `gulp-concat`, `uglify` as well as `jasmine` to have everything running smoothly.

With TypeScript, you can basically take out `jshint` out the window unless you want to lint your `.ts` files too.

So what does my gulp pipeline looks like?

0. rimraf
0. concat my files together
0. Run JSHint
0. uglify my file
0. Output to disk
0. Run my tests

With TypeScript, it look like:

0. rimraf
0. ~~concat my files together~~ (handled by the TypeScript compiler)
0. ~~Run JSHint~~ **Run TypeScript compiler** (no need to run JSHint on compiled code.)
0. uglify my file
0. Output to disk
0. Run my tests

The most basic of gulp task that compiles your TypeScript files is something like this:

``` javascript
var gulp = require('gulp');
var ts = require('gulp-typescript');

gulp.task('default', function () {
    return gulp.src('src/**/*.ts')
        .pipe(ts({
            noImplicitAny: true,
            out: 'output.js'
        }))
        .pipe(gulp.dest('built/local'));
});
```

### What should I do?

You should try it. Try to setup your workflow with TypeScript (which should be pretty easy).

Start with compiling your standard JavaScript code against TypeScript then try to move to creating `.ts` files.

Like my frameworks or languages in this world, you can't say you don't like it if you haven't tried it.

If you want to try it, here's how to install it.

### IDE Support

#### Visual Studio & VS Code

TypeScript is automatically installed with the lastest web tools. If you are doing ASP.NET MVC, you already have the tools installed.

#### Atom

To enable TypeScript support in Atom run the following:

```none
apm install atom-typescript
```

#### Sublime

With Package Control, install the [`TypeScript`](https://packagecontrol.io/packages/TypeScript) package.

For additional info on how to install TypeScript on Sublime, check out the [installation instructions](https://github.com/Microsoft/TypeScript-Sublime-Plugin#installation).

[1]: https://blogs.msdn.microsoft.com/typescript/2016/07/11/announcing-typescript-2-0-beta/
