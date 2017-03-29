---
title : "What's new in VS2017 and C# 7.0? - Out Variables"
date: 2017-03-29 09:00
tags: [c#, visual studio, .net]
---

When using certain APIs, some parameters are declared as `out` parameters.

A good example of this is the `Int32.TryParse(string, out int)` method.

So let's check the difference in invocation between C# 6.0 and C# 7.0

### C# 6.0

```csharp
public void DoSomething(string parameter)
{
    int result;
    if(Int32.TryParse(parameter, out result))
    {
        Console.WriteLine($"Parameter is an int and was parsed to {result}");
    }
}
```

### C# 7.0

```csharp
public void DoSomething(string parameter)
{
    if(Int32.TryParse(parameter, out int result))
    {
        Console.WriteLine($"Parameter is an int and was parsed to {result}");
    }
}
```

### The difference

Now you don't need to define the variable on a separate row. You can inline it directly and, in fact, you could just use `var` instead of `int` in the previous example since it can infer the type directly inline.

It is important to note however that the variable is scoped on the method and not the `if` itself. So the `result` parameter is available in both the `if` and the `else` scope.

### Are you going to use it?

When new features are introduced in a language, I like to ask people whether it's a feature they would use.

So please leave me a comment and let me know if you are going to use inline out variables. 
