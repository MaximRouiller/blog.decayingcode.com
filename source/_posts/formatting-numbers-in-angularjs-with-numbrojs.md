---
title: "Formatting numbers in AngularJS with Numbrojs"
date: 2016-06-17 09:00:00
tags: [angularjs, numbrojs]
---

When building an application, we always end up wanting to format our data in a certain way.

The issue happens when we try to integrate it with frameworks and their own opinionated way of coding.

AngularJS include a very interesting concept called `Filter` that allows us to integrate some of those frameworks really easily.

### What's a filter?

A filter in AngularJS allows you to *pipe* data to a function and return something.

A good example is taking a raw number and formatting to display it as currency or taking a date and showing it off in the user's region.

For more info about filters, check out [Angular's guide to filters][filters].

### What is Numbro?

[Numbro][numbro] is number formatting library that allows you to format/unformat numbers in different formats. Whether it's percentages, time, currency, or even bytes, Numbro got you covered.

### Unify both

Numbro, when added to your project, will add itself to the global scope. So how do you integrate it with Angular?

### Using filters

My favorite way to use it is to create a filter. That allows me to set general format to all my data and have a single point of change.

Here's a few examples

#### Byte formatting
```javascript
angular.module('FilterModule', [])
    .filter('bytes', function () {
        return function (bytes) {
            return numbro(bytes).format('0.0 b');
        };
})
```
```html
<div>
    <span>{{ file.size | bytes}}</span>
</div>
```

#### Currency formatting
```javascript
angular.module('FilterModule', [])
    .filter('currency', function () {
        return function (money) {
            return numbro(money).formatCurrency();
        };
})
```
```html
<div>
    <span>{{ invoice.amount | currency}}</span>
</div>
```

#### Percentage formatting
```javascript
angular.module('FilterModule', [])
    .filter('percentage', function () {
        return function (number) {
            return numbro(number).format('0 %');
        };
})
```
```html
<div>
    <span>{{ invoice.paidAmount / invoice.amount  | percentage}}</span>
</div>
```

### Formatting data the easy way

Of course that's just for numbers. You could do the same with dates with [momentjs][momentjs].

Just another reason why filters can make your life very easy for you as a developer.

[filters]: https://docs.angularjs.org/guide/filter
[numbro]:  http://numbrojs.com/
[momentjs]: http://momentjs.com
