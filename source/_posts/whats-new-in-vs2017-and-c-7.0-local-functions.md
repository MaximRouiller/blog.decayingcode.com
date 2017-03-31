---
title : "What's new in VS2017 and C# 7.0? - Local Functions"
date: 2017-03-31 09:00
tags: [c#, visual studio, .net]
---

Local Functions is all about declaring functions within functions. See them as normal functions with a more restrictive scope than `private.`

In C# 6.0, if you needed to declare a method within a method to be used within that method exclusively, you created a `Func` or `Action`.

#### C# 6.0 - Local Functions (before)

But here are some issues with `Func<>`/`Action<>`, first they are an object. Not a function. Every time you declare a Func, you allocate memory for the variable and that could put unnecessary pressure in your environment.

Second, `Func` cannot call themselves (also know as recursion) and finally, they have to be declared before you use them just like any variables.

```csharp
public void Something(object t)
{
    // allocate memory
    Func<object> a = () => return new object();

    Func<int, int> b = (i) => {
        //do something important
        return b(i); // <=== ILLEGAL. Can't invoke itself.
    };
}
```

Here's the problem with those however... if you do not want to allocate the memory or if you need to du recursion, you need to move the method to an external method and scope it properly (`private`).

Doing that however, your method becomes available to the whole class to use. That's not good.

#### C# 7.0 - Local Functions

```csharp
public bool Something(object t)
{
    return MyFunction(t);


    bool MyFunction(object t)
    {
        // return value based on `t`
    }
}
```

This is how a local function is declared in C# 7.0. It works the same way as a lambda but without allocation and without exposing private functions that shouldn't be exposed.

#### The difference

The main difference are:

* No memory allocation. Pure function that is just ready to be invoked and won't be reallocated every time the method is called.
* Recurse as much as you want. Since it's a normal method, you can use recursion just like any other methods.
* Use it before declaring it. Just like any other methods in a class, you can use it before (as in: line of codes) it is actually declared. Variables need to be declared before they are used.

Quickly, see it as normal method with a more aggressive scoping than `private`.

### Are you going to use it?

When new features are introduced in a language, I like to ask people whether it's a feature they would use.

So please leave me a comment and let me know if it's something that will simplify your life or, at least, your code.
