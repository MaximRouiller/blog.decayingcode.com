---
title : "What's new in VS2017 and C# 7.0? - Throw Expression"
date: 2017-03-31 09:02
tags: [c#, visual studio, .net]
---

`throw` has been a keyword since the first version of C#. The way the developer interact with it hasn't been touched ever since.

Sure, lots of new features has been brought on board since but... `throw`? Never touched until now.

#### C# 6.0

I do not really need to tell you how to throw an exception. There's 2 ways.

```csharp
public void Something()
{
    try
    {
        // throwing an exception
        throw new Exception();
    }
    catch(Exception)
    {
        // re-throw an exception to preserve the stack trace
        throw;
    }
}
```

And that was it. If you wanted to throw an exception any other way,

#### C# 7.0

All you see below are invalid before C# 7.0.

```csharp
public class Dummy
{
    private string _name;

    public Dummy(string name) => _name = name ?? throw ArgumentNullException(nameof(name));
    public void Something(string t)
    {
        Action act = () => throw new Exception();
        var nonNullValue = t ?? throw new ArgumentNullException();
        var anotherNonNullValue = t != null ? t : throw new ArgumentNullException();    
    }

    public string GetName() => return _name;
}
```

#### The difference

Oh so many of them. Here's what was included in the previous snippet of code.

You can now throw from:

* [Null-Coalescing operator](https://msdn.microsoft.com/en-us/library/ms173224.aspx). The `??` operator used to provide an alternative value. Now you can use it throw when value shouldn't be null.
* Lambda. Still don't understand exactly why it wasn't allow before but now? Totally legit.
* [Conditional Operator](https://msdn.microsoft.com/en-us/library/ty67wk28.aspx). Throw from the left or the right of the `?:` operator anytime you feel like it. Before? Not allowed.
* Expression Body. In fact, any expression body will support it.

Where you still can't throw (that I verified):

* `if(condition)` statement. Cannot be used in the condition of a `if` statement. Even if it's using a null-coalescing operator, it won't work and is not valid syntax.

### Are you going to use it?

I know that not everyone will necessarily use all of these new form of expression body. But I'm interested in your opinion.

So please leave me a comment and let me know if it's something that will simplify your life or, at least, your code.
