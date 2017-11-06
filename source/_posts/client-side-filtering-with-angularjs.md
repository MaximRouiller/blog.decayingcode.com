---
title: "Client side filtering with angularjs"
date: 2016-06-30 08:30:00
tags: [angularjs, javascript]
---

I always forget how to handle lists in Angular. So for my own sake and the benefit of my brain, I'll write it down here so that I have a chance to memorize it properly.

We always have arrays and we often want them in arrays. So let's start with the easy stuff.

### Note

Client filtering shouldn't be done with large dataset. Keep those on the server and filter it there. More performant for the user and less amount of data transferred to the user.

### Rendering a table from an array

Let's start with a simple example of an array being rendered into a table.

If I have a few items, that's perfect. But what if `items` can be an array of 100 items?

```html
<table>
    <thead>...</thead>
    <tbody>
        <tr ng-repeat="item in items">...</tr>
    </tbody>
</table>
```

Now, how do we filter this list to make it less massive for a user to look at.

### Creating a custom list filter

In Angular, it's quite easy to do. You just create a function in your `$scope` and invoke it with the [`filter` filter][filter]. There's other ways to use the filter but I'd rather keep my code in my code.

```html
<tr ng-repeat="item in items | filter: ItemFilter"></tr>
```

The function will expect `value`, `index` and `array` in their parameters.

```javascript
function ItemFilter(value, index, array){
    return value.isEnabled;
}
```

In our scenario, it would display all enabled items. But what if, our list need to be restricted to fewer elements?

### Limiting the amount of element in a list

There again, there's a filter for that! The [`limitTo`][limitTo] filter will take the X elements from your array. If the number is positive, it will be from the start of the array. If it's negative, it will be from the end of the array.

```html
<tr ng-repeat="item in items | filter: ItemFilter | limitTo: 10"></tr>
```

### That's it.

Now I'm just hoping that doing client side filtering in Angular will stick in my brain for just long enough for Angular2 to wipe it all away.


[filter]: https://docs.angularjs.org/api/ng/filter/filter
[limitTo]: https://docs.angularjs.org/api/ng/filter/limitTo
