---
title: "Report a Bug - The feature all products should have"
date: 2009-04-13 17:28:00
tags: []
---

I was writing acceptance test for project today and I needed to include a file inside my Excel 2007 sheet. After fiddling around a little, I found "Insert Object". This allowed me to insert a file and have it available to anyone opening the Excel 2007 document. When I dragged and dropped the file to my desktop, the file was truncated! I couldn't believe it! I found a bug in Excel 2007! Being a developer, I wanted to properly report that bug.

So I started with a small Google search for "report Microsoft bug" which lead me to [this](http://weblog.timaltman.com/archive/2006/03/22/reporting-bugs-microsoft), [this](http://blogs.msdn.com/astebner/archive/2005/08/13/451338.aspx) and [that](http://www.oreillynet.com/mac/blog/2002/06/mission_impossible_submitting.html).

Let me bring you the title of the last article : **"Mission: Impossible. Submitting a Bug Report to Microsoft"**.

This had me afraid. The article is dated 2002 and the other ones are 2006\. We are in 2009 at the time of writing this article. So I searched more and more. I literally found **nothing**.

How can an application ship without having an easy way to report a bug? [Apple does it](http://www.apple.com/feedback/macosx.html). [Ubuntu does it](https://help.ubuntu.com/community/ReportingBugs). [OpenOffice does it](http://qa.openoffice.org/issue_handling/submission_gateway.html). [Firefox does it](https://developer.mozilla.org/en/Bug_writing_guidelines#Reporting_a_New_Bug). [Google Chrome does it](http://www.google.com/support/chrome/bin/answer.py?hl=en&amp;answer=95760). Why can't I report a bug for Internet Explorer, Office or Vista (soon Windows 7)?

It goes without mention that if you are building a product, there should be an easy way to report a bug. Be it by a small contact form, an official bug reporting software, etc. But please don't [charge your user 35$ to fill out bug report](http://weblog.timaltman.com/archive/2006/03/22/reporting-bugs-microsoft).

Microsoft has recently (in the last few years) opened up and launched [Connect](http://connect.microsoft.com) to help user report bug inside the .NET Framework. They should extend it to all their product line. It seems that developer working on the .NET framework are reachable and have blogs everywhere and that the Excel team is just higher up in the sky and untouchable by customer feedback.

So? Does your application have a way to easily report bug? Because if the process to report a bug is long and tedious, you will have a "_bug-less_" product with marginal amount of users that tolerates the bug or that are hard-core enough to take the time to report it.

