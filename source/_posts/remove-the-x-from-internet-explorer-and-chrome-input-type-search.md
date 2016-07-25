---
title : "Remove the X from Internet Explorer and Chrome input type search"
date: 2016-07-25 09:00
tags: [css, html]
---

When you have a some input with `type="search"`, typing some content will display an `X` to allow you to clear the content of the box.

```html
<input type="search" />
```

That `X` is not part of Bootstrap or any other CSS framework. It's built-in the browser.

The only way to remove it is to apply something like this:


```css
/* clears the 'X' from Internet Explorer */
input[type=search]::-ms-clear {  display: none; width : 0; height: 0; }
input[type=search]::-ms-reveal {  display: none; width : 0; height: 0; }

/* clears the 'X' from Chrome */
input[type="search"]::-webkit-search-decoration,
input[type="search"]::-webkit-search-cancel-button,
input[type="search"]::-webkit-search-results-button,
input[type="search"]::-webkit-search-results-decoration { display: none; }
```

That's it. Copy/paste that in our main CSS file and all search box won't have that anoying X anymore.
