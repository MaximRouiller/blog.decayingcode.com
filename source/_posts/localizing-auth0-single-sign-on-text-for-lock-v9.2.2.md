---
title : "Localizing Auth0 Single Sign-on text for Lock v9.2.2"
date: 2016-07-28 10:00:00
tags: [angularjs, auth0, css]
---

In the current mandate, we are using Auth0 Lock UI v9.2.2.

There happen to be a bug with the widget where the following text is hardcoded.

> Single Sign-on Enabled

To fix this issue in our AngularJS Single Page Application, we had to introduce this in the `index.html` file.

```html
<style ng-if="myLanguage == '<LANG HERE>'">
   .a0-sso-notice::before{
       font-size: 10px;
       content: '<YOUR TEXT HERE>';
   }
   .a0-sso-notice{
       /* hack to hide the text */
       font-size: 0px !important;
   }
</style>
```

This piece of code fixes the issue when a specific language is specified. `ng-if` will completely remove the tag when the language won't match and *monkey patch* the text.

If you have more than 2 languages, it would pay to consider injecting the text directly within the style tag. Since Angular doesn't allow you to parse it, [somebody else](http://alexbaden.me/interpreting-data-binding-in-style-tags-with-angular/) already documented how to do it.
