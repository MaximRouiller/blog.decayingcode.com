---
title : "What's new in VS2017 and C# 7.0? - Pattern Matching"
date: 2017-03-29 09:00
tags: [c#, visual studio, .net]
---

C# 7.0 introduces pattern matching. Well, compared to other features, this require a little bit of explanation.

There is many types of pattern matching and three are supported in C# 7: Type, Const, Var.

If you used the `is` keyword before, you know it test for a certain type. However, you still needed to cast the variable if you wanted to use it. That alone made the `is` statement completely irrelevant and people preferred to cast and check for null rather than check for types.

#### C# 6.0 - Type Pattern Matching (before)

```csharp
public void Something(object t)
{
    var str = t as string;
    if(str != null)
    {
        //do something
    }

    var type = t.GetType();

    if (type == typeof(string))
    {
        var s = t as string;
    }
    if (type == typeof(int))
    {
        var i = t as int;
    }
}
```

#### C# 7.0 - Type Pattern Matching

```csharp
public void Something(object t)
{
    if(str is string str)
    {
        //do something
    }

    switch(t)
    {
        case string s:
            break;
        case int i:
            break;
        //...
        case default:
            break;
    }
}
```

#### The difference with Type Pattern Matching

This saves you one line in a pattern that is common and repetitive way too often.

### More pattern matching

#### C# 7.0 - Const Pattern Matching

`Const` pattern is basically checking for specific value. That includes `null` check. Other constant may also be used.

```csharp
public void Something(object t)
{
    if(t is null){ /*...*/ }
    if(t is 42) { /*...*/}
    switch(t)
    {
        case null:
            // ...
            break;
    }
}
```

##### C# 7.0 - Var pattern Matching

This is a bit more weird and may look completely pointless since it will not match any types.

However, when you couple it with the `when` keyword... it where magic starts coming.

```csharp
private int[] invalidValues = [1,4,7,9];
public bool IsValid(int value)
{
    switch(value)
    {
        case var validValue when( !invalidValues.Contains(value)):
            break;
        case var invalidValue when( invalidValues.Contains(value)):
            break;
        case default:
            break;
    }
}
```

Of course, this example is trivial but add some real-life Line Of Business applications and you end up with a very versatile of putting incoming values into the proper bucket.



### Are you going to use it?

When new features are introduced in a language, I like to ask people whether it's a feature they would use.

So please leave me a comment and let me know if it's something that will simplify your life or, at least, your code.
