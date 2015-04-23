---
title: "PostSharp - The best way to do AOP in .NET"
date: 2009-06-13 20:33:00
tags: []
---

Who knows about [Aspect-Oriented Programming](http://en.wikipedia.org/wiki/Aspect-oriented_programming) (AOP)? Common! Don't be shy! Ok, now lower your hands. My prediction is that a lot of you didn't raise their hands. So let's resume what AOP is:

> Aspect-oriented programming is a programming paradigm that **increases modularity** by enabling **improved separation of concerns**. This entails breaking down a program into distinct parts (so called concerns, cohesive areas of functionality). [...]

So what does it mean truly? Well, it's a way to declare part of your software (methods, classes, assembly) to have a "concern" applied to them. What is a concern? Logging is one. Exception handling is another one. But... let's go wild... what about caching, impersonation, validation (null check, bound check), are all **concerns**. Do you mix them with your code? Right now... you are forced to do it.

##### The state of current AOP

Alright for those who raised their hands earlier, what are you using for your AOP concerns? If you are using patterns and practices Policy Injection module, well, you are probably not happy. First, all your objects need to be constructed by an object builder and need to inherit from **MarshalByRefObject** or implement an interface.

This is not the best way but it's been done in the "proper" way without hack.

##### What is PostSharp bringing?

PostSharp might be a "hack" if think so. Of course, it does require you to have it installed on your machine while compiling for it to work. But... what does PostSharp does exactly? It does what every AOP should do. Inject the code before and after the matching method at compile time. Not just PostSharp methods but any methods that is inherited from the base class PostSharp is ofering you. Imagine what you could do if you could tell the compiler to inject **ANY** code before/after your method on **ANY** code you compile. Think of the possibilities. I'll give you 2 minutes for all this information to sink in... (waiting)... got it? Start to see the possibility? All you need to do is put attributes on your methods/attributes like this:

```cs
[NotNullOrEmpty]
public string Name { get; set; }

[Minimum(0)]
public int Age { get; set; }
```

Now look at that code and ask yourself what it do exactly. Shouldn't be hard. The properties won't allow any number under "0" to be inserted inside "Age" and "Name" will not allow any null or empty string. If there is any code that try to do that, it will throw a ValidationException.

##### Wanna try it?

Go [download PostSharp immediatly](http://www.postsharp.org/) and it's little friend [ValidationAspects on Codeplex](http://www.codeplex.com/ValidationAspects). After you have tried, try to build your own and start cleaning your code to achieve better readability.

And yes... both are Open-Source and can be used at no fee anywhere in your company.

##### Suggestion to CLR Team

Now, PostSharp force us to have it installed with the MSI for it to work because it needs to install a Post-Compile code injector (like some obfuscation tools). What would be **<span style="text-decoration: underline;">really</span>** nice, is to be able to do the same thing built-in with the compiler. The compiler is already checking for some attribute already... I would love to have this "internal working" exposed to the public so that we can build better tools and, more importantly, better code.

**UPDATE:** I want to mention that PostSharp is **NOT** open-source. However is free unless you need to package it with your tool.
