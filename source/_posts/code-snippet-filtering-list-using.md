---
title: "Code Snippet: Filtering a list using Lambda without a loop"
date: 2009-01-20 15:42:00
tags: [c#,snippet]
---

This is not the latest news of the day... but if you are doing filtering with loops, you are doing it wrong.

Here's a sample code that will replace a loop easily:

```cs
List<String> myList = new List<string> {"Maxim", "Joe", "Obama"};
myList = myList.Where(item => item.Equals("Obama")).ToList();
```

While this might sound too easy, it's easily done and works fine. Just make sure that the objects that you are using inside this kind of filtering are not ActiveRecord DAL object or you might be sorry for the performance.
