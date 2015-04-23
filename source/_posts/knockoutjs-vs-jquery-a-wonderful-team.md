---
title: "KnockoutJS vs jQuery – A wonderful team"
date: 2013-05-09 15:30:00
tags: [javascript]
---

KnockoutJS is an MVVM JavaScript framework for your browser. It allows you to easily bind raw data to a model and update elements bound to that model.

jQuery is a DOM manipulation framework that has made JavaScript not suck. 

Both have their reason to exist and they should actually not compete. It’s all about using the right tool for the right job.

### The problem

When building rich HTML page with a lot of input/tag manipulation, the most common manipulation is changing what is displayed. Be it the content of a text box or the content of a tag, we need to update those elements a lot. What we often end up is a lot of jQuery code that selects element and then updates it.

It becomes very quickly clear that increasing the amount of code would make it look like spaghetti code. Another big disadvantage of having all of your UI driven by jQuery is that you end up with a lot of selectors. Unless you are selecting by ID, tag or classes (and depending on your browser), selectors will eventually slow your browser. 

So how do we fix spaghetti event binding and keep our head cool?

### Fixing the problem with KnockoutJS

Let’s start with some basic HTML:

```xml
<div id="myModel">    
    <label>Firstname</label>
    <input type="text" id="firstname" />

    <label>Lastname</label>
    <input type="text" id="lastname" />

    <span><strong id="fullname">Displaying full name here</strong></span>
</div>
```

That’s the most simplistic example. We have a first name and a last name and we want to concatenate both of them into an tag to display the user and that, in real time. This might seem like an easy problem but keep in mind that real-life problems will actually be more fierce. With that said, let’s start coding.

### What would that look like in jQuery?

```js
$(document).ready(function () {
    $("#firstname").on('keyup', UpdateFullname);
    $("#lastname").on('keyup', UpdateFullname);
});

function UpdateFullname() {
    $("#fullname").text($("#firstname").val() + ' ' + $("#lastname").val());
}
```

We basically have no less than 5 selectors. All based on IDs so they are going to be fast but still… it’s five selectors. We could probably optimize things a bit but this is as close as production code I’ve seen in the wild. The trick for the real-time requirement in this scenario is the ‘keyup’ event. As we add more elements to our model we might have to add more event binding to invoke that function on other elements. Maybe other functions will require that same function too and you end-up with a flurry of selectors left and right with a big JavaScript file of 800 lines of code in no times.

### What about KnockoutJS?

```js
$(document).ready(function () {
    ko.applyBindings(new FullnameViewModel(), $("#myModel")[0]);
});

function FullnameViewModel() {
    var self = this;
    self.firstName = ko.observable('');
    self.lastName = ko.observable('');

    self.fullname = ko.computed(function() {
        return self.firstName() + ' ' + self.lastName();
    });
}
```

Of course, for the KnockoutJS version to work properly, I have to change the HTML a little bit. I’ll also take the time to remove unused attributes (required by jQuery in this case) to work. Here is how it looks now.


```xml
<div id="myModel">    
    <label>Firstname</label>
    <input type="text" data-bind="value: firstName, valueUpdate: 'afterkeydown'" />

    <label>Lastname</label>
    <input type="text" data-bind="value: lastName, valueUpdate: 'afterkeydown'" />

    <span><strong data-bind="text: fullname">Displaying full name here</strong></span>
</div>
```

So what can we understand from that? Yes, it takes a bit more JavaScript to do the work but now, the whole “business rules” are actually encapsulated in one JavaScript function. If we need to reuse part of how the full name is built, it’s actually part of your model. It’s something you can write tests for. As more rules are added to the view model, less time is spent debugging which selector I am to use and more about writing business rules of our presentation layer.

### Why should I go with Knockout?

*   If your application actually has some business rules that need encapsulating, Knockout will provide you an easy way to do it.*   If you are unit testing your JavaScript, it is much faster and easier to test only the ViewModel without any actual HTML in the back. You could potentially run something like PhantomJS to test your ViewModels.
*   If you are going to reuse part of the model in other bits of HTML or simply if your HTML is still changing a lot
*   If you need to be able to serialize your whole model in JSON to send to the server.

### What should I still use jQuery for?

*   Basic DOM manipulation/selection
*   Ajax requests
*   Effects and animation

### Give KnockoutJS a try!

Try it out by going first to [KnockoutJS.com](http://ww.knockoutjs.com) and doing the live tutorial. It will be easy to get your feet wet. Then, use it in Visual Studio since it’s part of the template!
