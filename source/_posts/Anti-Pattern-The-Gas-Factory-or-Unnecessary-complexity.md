---
title: "Anti-Pattern: The Gas Factory or Unnecessary complexity"
date: 2009-03-29 21:47:00
tags: []
---

Just as in any system, when you start coding some structure, you always try to make it as generic as possible to make it easy to later reuse those parts.[![Just like petrolum complex, things starts getting complex](http://lh3.ggpht.com/_S0YTV7NEdrk/SdAkn3brhtI/AAAAAAAAAEA/XHhf8X6GlS4/petrol_complex_thumb%5B13%5D.jpg?imgmax=800 "Just like petrolum complex, things starts getting complex")](http://lh6.ggpht.com/_S0YTV7NEdrk/SdAknnVXYyI/AAAAAAAAAD8/zxlxveGSE-k/s1600-h/petrol_complex%5B15%5D.jpg) There is normal complexity when you build your code and as you go, complexity adds up. However, one of the main problem of this anti-pattern is when it's done consciously.

Let's give a quick example here. You start building collections and one of your collection need a special feature. The problem is when you extract this functionality and try to make it as generic as possible so that anyone could reuse your class. **That** is the problem of the unnecessary complexity. You see, as you are making stuff generic for a single class, you are adding structures inside your code. Some of those structure might be proof tested for this specific class but might fail on other classes. There is also the problem that maybe no other class will **ever** require this functionality.

#### How do I solve this anti-pattern?

By following [YAGNI](http://en.wikipedia.org/wiki/You_Ain "You Ain") and [Lean Software Development](http://en.wikipedia.org/wiki/Lean_software_development), you delay code and unnecessary complexity until you actually require the complexity. If&nbsp; you have 2 classes that require the same functionality, it is now time to extract this functionality inside a different class and make those 2 classes inherit from it (or any other patterns that are required).

And here, I'm not just talking about inheritance. I'm also talking about unnecessary design patterns. If you built a pipeline component to calculate discount but you only have one discount at the moment, it might be actually relevant to implement it anyway since you are sure you might require it later. However, the client is the one that is supposed to drive the requirements and if you don't require the pipeline immediately... well... don't built it!

It doesn't mean to leave your code in a fixed state. It just means to keep your code clean to ease the implementation of the pipeline.

The best line of codes are those we don't need to write.