---
title : "Angular 1.5+ with dependency injection and uglifyjs"
date: 2017-02-01 09:30
tags: [angularjs, javascript]
---

Here's a problem that doesn't come too often.

You build your own build pipeline with AngularJS and you end-up going in production with your development version. Everything runs fine.

Then you try your uglified version and... it fails. For the fix, skip to the end of the article. Otherwise? Keep on reading.

### The Problem

Here's some Stack Trace you might have in your console.

> Failed to instantiate module myApp due to: <br/>
> Error: [$injector:unpr]  http://errors.angularjs.org/1.5.8/$injector/unpr?p0=e

and this link shows you this:

> Unknown provider: e

### Our Context

Now... in a sample app, it's easy. You have few dependencies and finding them will make you go through a few files at most.

My scenario was in an application with multiple developers after many months of development. Things got a bit sloppy and we made decisions to go faster.

We already had practices in place to require developers to use explicit dependency injection instead of implicit. However, we didn't have anything but good faith in place. Nothing against human mistake or laziness.

### Implicit vs Explicit

Here's an implicit injection

```javascript
angular.module('myApp')
    .run(function($rootScope){
        //TODO: write code
    });
```

Here's what it looks like explicitly (inline version)

```javascript
angular.module('myApp')
    .run(['$rootScope', function($rootScope){
        //TODO: write code
    }]);
```

### Why is it a problem?

When UglifyJS will minify your code, it will change variable names. Names that AngularJS won't be able to match to a specific provider/injectable. That will cause the problem we have where it can't find the right thing to inject. One thing that UglifyJS won't touch however is strings. so the `'$rootScope'` present in the previous tidbit of code will stay. Angular will be able to find the proper dependency to inject. And that, even after the variable names get mangled.

### The Fix

`ng-strict-di` will basically fails anytime it finds an implicit declaration. Make sure to put that into your main Angular template. It will save you tons of trouble.

```html
<html ng-app="myApp" ng-strict-di>
...
</html>
```

Instead of receiving the cryptic error from before, we'll receive something similar to this:

> Uncaught Error: [$injector:modulerr] Failed to instantiate module myApp due to:<br/>
Error: [$injector:strictdi] function(injectables) is not using explicit annotation and cannot be invoked in strict mode
