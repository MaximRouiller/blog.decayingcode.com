---
title: "Ajax with jQuery and ASP.NET MVC"
date: 2011-03-14 10:54:40
tags: [jquery, javascript, asp.net]
---

# Requirements

*   ASP.NET MVC 2 (or higher)*   jQuery (latest version or the one shipped with MVC)*   [http://docs.jquery.com](http://docs.jquery.com)  

# What should I learn first?

When doing AJAX with ASP.NET MVC, there is no “UpdatePanel” or high level abstraction that helps you do all the magic. You need to get your hands dirty. That means learning how to do actual JavaScript without having the framework do all the job for you.

My favorite tool is jQuery when it comes to JavaScript. It’s simple, small and allows you to do in a few line of codes in 5 minutes what would take me 3 days an 2000 lines of code.

What should be on your reading list?

## Document Ready Event

First, do not forget to put any jQuery code inside the following snippet:

```js
$(document).ready(function () {
    // all code need to go there.
});
```

## [Selectors](http://api.jquery.com/category/selectors/)

The three most important selectors (in my opinion) are the following : 

*   [ID selector](http://api.jquery.com/id-selector/)
*   [Class selector](http://api.jquery.com/class-selector/)
*   [Attribute selector](http://api.jquery.com/attribute-equals-selector/)

Those three selectors will include around 80 to 90% of all the selector you will require. The others, you can pick up along the way!

## [Manipulation](http://api.jquery.com/category/manipulation/)

Here, we are talking about manipulating elements from the HTML page. We will need those since they are required for removing and adding HTML into the page. It’s basically what the UpdatePanel for WebForms do but we’ll do it manually and be more specific in what we want. 

Here what I consider essential :

*   [remove()](http://api.jquery.com/remove/) – Allows you to remove an element from the DOM. This is useful when wanting to remove an element directly.
*   [append()](http://api.jquery.com/append/)/[prepend()](http://api.jquery.com/prepend/) – Allows you to insert an element inside the selected tags (either before or after the existing elements of that tag)
*   [replaceWith()](http://api.jquery.com/replaceWith/) – Replace the element with what’s given in parameters. When using this, make sure that the replaced element have the same usable selector or it will harder to reselect that element.

And now, we have nearly everything that we need to start being AJAX-y!

# I already know all that, let me do AJAX!

So what’s the easiest with jQuery to do AJAX?

## [$.get()](http://api.jquery.com/jQuery.get/)

This method will basically do an HTTP Get on the selected URL and return the data inside the success function callback. 

Let’s say I want to add the result of my AJAX request to the following tag: 

```xml
<div id='result'>
</div>
```

It would be as easy as doing this :

```js
$.get('/Controller/Action/', null, function (data) {
    $('#result').empty(); // clears the content
    $('#result').append(data); // append the data into the div
});
```

Wasn’t that easy? 

# The MVC Side

Of course, if your MVC Controller is returning an JsonResult, you could use `$.getJSON` instead and data would be an object instead of pure HTML. In fact, your controller can even return a simple string and it would work. But how do you make your controller returns only what you need? Actions can be split in 2 with MVC to respond differently whether the request is standard or an AJAX request. Here is what it would look like: 

```cs
public ActionResult MyAction() 
{
    if(Request.IsAjaxRequest())
        return View('nameOfMyPartialView');
    else
        return View();
}
```

That’s it! And now if you want to return an object as JSON:

```cs
public ActionResult AnotherAction()
{
    if(Request.IsAjaxRequest())
    {
        // The following line return Json to AJAX requests
        return Json(new { name = 'Maxime Rouiller'}, JsonRequestBehavior.AllowGet);
    }

    // we still return normal HTML to standard requests.
    return View();
}
```

I hope you enjoyed this small example. If you are still curious or if you are simply stuck with something, ask away!
