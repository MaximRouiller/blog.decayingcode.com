---
title: "How to insert page breaks in printed web pages using only CSS?"
date: 2012-01-05 06:44:31
tags: [css]
---

So one of my co-worker had to print a “report”. I say report but it’s more like they wanted the page printed but just more compact and less fluffy/pretty content. Like always, people tend to bring solutions instead of problems and they asked me how long it would take me to do that in SSRS.

Of course, I gave my evaluation on how long it should take but then… we stumbled into problems. My co-worker only have Visual Studio 2010 installed. SSRS requires you to have BIDS which uses Visual Studio 2008\. Then, we have SQL Server 2008 R2 installed on our machine but no-R2 on our deployment server. This quickly became a mess.

Then after taking the time to look at the problem, we asked: “Why couldn’t we just print the web page?”

Well for starters, the requirements we had was there was a need for a page break at certain predictable location. 

After searching a bit, I came up with this solution directly from W3C (and a bit other sources).
```css
@media print {
    .pagebreak {
        page-break-before: always;
    }
}
```

And that’s it. Just add the class to the element that should be on a new page and it works.

Now, what about compatibility you say?

It should be compatible with IE (all current versions down to 6), FireFox (all latest versions), Chrome (tested on v16) and Opera (all versions).