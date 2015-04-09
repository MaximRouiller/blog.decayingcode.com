---
title: "Better JavaScript notifications with Toastr"
date: 2013-05-07 09:55:00
tags: [javascript]
---

A use case I&rsquo;ve always been pondering about is how to create a small popup that will tell the user that an operation has succeeded or failed.

What I usually did is write by own Success/Error functions to handle displaying the message to the user. Integrate a bit of jQuery here, a bit of jQuery UI here, download a custom script there&hellip;

Recently, I&rsquo;ve found [Toastr](http://codeseven.github.io/toastr/). What is great about that tool is how it integrates with pretty much anything a designer can throw at you. CSS classes are replaceable, icons are defined inline in the CSS to avoid external dependencies on images, etc.

Basically, you don&rsquo;t need the CSS file that comes bundled with it. You only need the JS file if you already got the style all figured out.

Here is how I configured it on my end :

```js
toastr.options = {
    toastClass: 'myCustomDivClass',
    iconClasses: {
        error: 'error_popup',
        success: 'success_popup',
        info: 'info_popup',
        warning: 'warning_popup'
    },
    positionClass: '', // I position it properly already. not needed.
    fadeIn : 300, // .3 seconds
    fadeOut: 300, // .3 seconds
    timeOut: 2000, // 2 seconds - set to 0 for 'infinite'
    extendedTimeOut: 2000, // 2 seconds more if the user interact with it
    target: 'body'
};
```

Basically, this will configure a basic DIV tag element and append it to the of the BODY tag. This is very important in some browser because in some scenarios, form elements might appear over the &ldquo;toast&rdquo;.

Another basic boilerplate that you might want to add to your code is this:

```js
$.ajaxSetup({
    error: function() {
        toastr["error"]("&lt;h2&gt;An error has occured.&lt;/h2&gt;");
    }
});
```

This will ensure that any AJAX errors that happens within jQuery is handled using Toastr.

### Installing Toastr

[Nuget Package](http://nuget.org/packages/toastr) / [Source](https://github.com/CodeSeven/toastr)

**Nuget command line:** 
```ps
Install-Package toastr
```