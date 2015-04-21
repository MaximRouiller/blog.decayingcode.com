---
title: "TDD: How I applied TDD to a simple problem"
date: 2009-02-28 20:43:00
tags: [unit test]
---

A month or two ago I had to built a component that had to analyse a string a return some information out of it. The best result for this was a Regular Expression and I was sure. So I started writing what kind of input would be valid and which one should not be allowed.

When I started writing this code, I already read many blog posts about it and wanted to give it a try. Since I wanted a simple scenario which would easily be applied, when I had to parse this string I knew I had a simple problem to which I could apply it.

If anyone has ever worked with regular expressions, it is widely known that it's easy to make something match. However, it's really hard to make it match what you want and not what you don't want. For proof of that, try seeing how many regular expression there is to [parse a phone number](http://regexlib.com/Search.aspx?k=phone%20number). The non-matches is as important as the matches.

So I started a test project and added my first test. The test was to test the perfect scenario. Of course the test didn't even compiled. I then proceeded to create the missing classes and made the test compile. Of course, all the class had the following inside them:

```cs
throw new NotImplementedException();
```

This ensured that the test "went red". I then proceeded to implement the minimum necessary to make it pass and make it "go green". And I kept on rolling until it worked in all my specified cases. Sometimes, previous tests went and failed. Sometimes, everything stayed green but I kept on going.

For experienced TDD-er it's common and normal. But for me, it was weird. I made sure to follow EXACTLY what it said. When it said "minimum necessary to make it pass", I made sure that I returned a constant if I could or proceeded to create/modify the regular expression as needed. And as I kept increasing my amount of test, the constant all went away, the regular expression got more precise and every time that I broke a test I came back to fix it. It's a weird feeling but when I gave my class for usage, it was working as perfectly as it could.

So why the ruckus? Because since I wrote this piece of code, I haven't seen one bug report. The code is perfect for what it is doing. When a bug will come my way, I'll try the TDD-er way of doing thing. Add a test that reproduce the bug and fix my code to make sure it works on all test.

Result of everything? One bug ridden class, 20 something tests and one developer who learned the essence of TDD.

I would love to get my teeth on more complex code now.

