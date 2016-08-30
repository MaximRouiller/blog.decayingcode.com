---
title : "Ajax requests in a loop and other scoping problems with JavaScript"
date: 2016-08-30 12:00
tags: [javascript]
---

In the current code I'm working on, I had to iterate over an array and execute an Ajax request for each of these elements.

Then, I had to do an action on that element once the Ajax request resolved.

Let me show you what it looked like at first.

#### The code

```javascript
function Sample() {
    var products = [{name: "first product"}, {name: "second product"}, {name: "third product"}];

    for(var i = 0; i < products.length; i++) {
        var product = products[i];
        $.get("https://api.github.com").then(function(){
            console.log(product.name);
        });
    }
}
```
**NOTE:** I'm targeting a GitHub API for demo purposes only. Nothing to do with the actual project.

So nothing is especially bad. Nothing is flagged in JSHint and I'm expecting to have the product names displayed sequentially.

#### Expected output
```none
first product
second product
third product
```

Commit and deploy, right? Well, no.

Here's what I got instead.

#### Actual output
```none
third product
third product
third product
```

What the hell is going here?

### Explanation of the issue

First, everything has to do with [scope](http://stackoverflow.com/a/500459/24975). `var` are scoped to the closest `function` definition or the global scope depending on where the code is being executed. In our example, var is scoped to a function above the `for` loop.

Then, we have the fact that variables can be declared multiple times and only the last value is taken into account. So every time we loop, we redefine `product` to be the current instance of `products[i]`.

By the time an Http request comes back, `product` has already been (re-)defined 3 times and it only takes into account the last value.

Here's a quick timeline:

0. Start loop
0. Declare `product` and initialize with `products[0]`
0. Start http request 1.
0. Declare `product` and initialize with `products[1]`
0. Start http request 2.
0. Declare `product` and initialize with `products[2]`
0. Start http request 3.
0. Resolve HTTP Request 1
0. Resolve HTTP Request 2
0. Resolve HTTP Request 3

HTTP requests are asynchronous slow operations and will only resolve after the local code is finished executing. The side-effect is that our `product` has been redefined by the time the first request comes back.

Ouch. We need to fix that.

### Fixing the issue the old way

If you are coding for the browser in 2016, you want to use closures. Basically, passing the current value in a function that is executed when defined. That function will return the appropriate function to execute. That solves your scope issue.

```javascript
function Sample() {
    var products = [{name: "first product"}, {name: "second product"}, {name: "third product"}];

    for(var i = 0; i < products.length; i++) {
        var product = products[i];
        $.get("https://api.github.com").then(function(product){
                return function(){
                console.log(product.name);
            }
        }(product));
    }
}
```

### Fixing the issue the new way

If you are using a transpiler like [BabelJS](https://babeljs.io/), you might want to use ES6 with `let` variable instead.

Their scoping is different and way more sane than their `var` equivalent.

You can see on [BabelJS][1] and [TypeScript][2] that the actual problem was resolved in a similar way.

```javascript
function Sample() {
    var products = [{name: "first product"}, {name: "second product"}, {name: "third product"}];

    for(var i = 0; i < products.length; i++) {
        let product = products[i];
        $.get("https://api.github.com").then(function(){
            console.log(product.name);
        });
    }
}
```

### Time for me to use a transpiler?

I don't know if, for me, it's the straw that will break the camel's back. I'm really starting to consider using a transpiler that will make our code more readable and less buggy.

This is definitely going on my TODO list.

What about you guys? Have you encountered bugs that would not have happened with a transpiler? Leave a comment!

[1]: https://babeljs.io/repl/#?babili=false&evaluate=true&lineWrap=false&presets=es2015%2Creact%2Cstage-2&code=function%20Sample()%20%7B%0D%0A%20%20%20%20var%20products%20%3D%20%5B%7Bname%3A%20%22first%20product%22%7D%2C%20%7Bname%3A%20%22second%20product%22%7D%2C%20%7Bname%3A%20%22third%20product%22%7D%5D%3B%0D%0A%0D%0A%20%20%20%20for(var%20i%20%3D%200%3B%20i%20%3C%20products.length%3B%20i%2B%2B)%20%7B%0D%0A%20%20%20%20%20%20%20%20let%20product%20%3D%20products%5Bi%5D%3B%0D%0A%20%20%20%20%20%20%20%20%24.get(%22https%3A%2F%2Fapi.github.com%22).then(function()%7B%0D%0A%20%20%20%20%20%20%20%20%20%20%20%20console.log(product.name)%3B%0D%0A%20%20%20%20%20%20%20%20%7D)%3B%0D%0A%20%20%20%20%7D%0D%0A%7D
[2]: http://www.typescriptlang.org/play/index.html#src=function%20Sample()%20%7B%0D%0A%20%20%20%20var%20products%20%3D%20%5B%7Bname%3A%20%22first%20product%22%7D%2C%20%7Bname%3A%20%22second%20product%22%7D%2C%20%7Bname%3A%20%22third%20product%22%7D%5D%3B%0D%0A%0D%0A%20%20%20%20for(var%20i%20%3D%200%3B%20i%20%3C%20products.length%3B%20i%2B%2B)%20%7B%0D%0A%20%20%20%20%20%20%20%20let%20product%20%3D%20products%5Bi%5D%3B%0D%0A%20%20%20%20%20%20%20%20%24.get(%22https%3A%2F%2Fapi.github.com%22).then(function()%7B%0D%0A%20%20%20%20%20%20%20%20%20%20%20%20console.log(product.name)%3B%0D%0A%20%20%20%20%20%20%20%20%7D)%3B%0D%0A%20%20%20%20%7D%0D%0A%7D
