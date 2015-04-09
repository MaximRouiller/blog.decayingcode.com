---
title: "Introduction to NSubstitute"
date: 2013-05-08 04:00:00
tags: [c#,mock]
---

[Moq](https://code.google.com/p/moq/) has been for a long time my favorite tool to do mocking. Today, I'll be introducing another tool I've recently start to use.

While Moq was more based around creating a mock, configuring it and then retrieving the object, NSubstitute is more about creating the object and then configuring it.

In NSubstitute, there is no direct concepts of Mock that is presented to the user.

As an example, here is a simple way to do a mock:

```cs
IProductRepository repository = Substitute.For<IProductRepository>();
```

That's it. As of this moment, you have a mocked repository. Injecting it is very simple at that point.

Since we don't have a Mock object to work with, how do we go and configure our mock to return what we want?

NSubstitute include an already familiar concept called "Extension Methods" to our users. The one that is going to be used the most is "Returns". There is two available signature for Returns. Let's see the decompiled signature for "Returns".

```cs
public static ConfiguredCall Returns<T>(this T value, Func<CallInfo, T> returnThis, params Func<CallInfo, T>[] returnThese)
public static ConfiguredCall Returns<T>(this T value, T returnThis, params T[] returnThese)
```

You return an instance that has already been built or you return a function that will build (or return) the instance at the moment of invocation. As for the array at the end, those are what is going to be returned/invoked in subsequent calls.

That's really as simple as it get. Since mocking framework are supposed to be easy to use and not get in your way, I think that NSubstitute might have just taken the place of Moq for me as my go-to framework from now on. Simple, lightweight&hellip; I like it!

### Installing NSubstitute

[Website](http://nsubstitute.github.io/) / [Source](https://github.com/nsubstitute/NSubstitute) / [Nuget](http://nuget.org/packages/NSubstitute)

**Nuget command line:** 

```ps
Install-Package NSubstitute
```