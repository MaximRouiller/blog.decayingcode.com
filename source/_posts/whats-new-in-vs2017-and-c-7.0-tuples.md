---
title : "What's new in VS2017 and C# 7.0? - Tuples"
date: 2017-03-29 20:20
tags: [c#, visual studio, .net]
---

C# 7.0 introduces tuples for the first time. Although present in many other languages and introduced first as a generic type (`System.Tuple<...>`) before, it wasn't until C# 7.0 that it was actually included into the language specification.

The raison-d'être of the Tuple is to return 2 values at the same time from a method.

#### C# 6.0

Many solutions were provided to us before.

We could use:
* Out-Parameters but they are usable in async methods so it's one solution.
* `System.Tuple<...>` but just like `Nullable<...>`, it's very verbose.
* Custom type. But now you are creating classes that will never be reused ever again.
* Anonymous types. But you were required to use dynamic which add a huge performance overheard everytime it's used.

#### C# 7.0 - Defining tuples

The simplest use is like this:

```csharp
public (string, string) Something()
{
    // returns a literal tuple of strings
    return ("Hello", "World");
}

public void UsingIt()
{
    var value = Something();
    Console.WriteLine($"{value.Item1} {value.Item2}")
}
```

Why stop there? If you don't want to return `ItemX` as their name, you can customize it two different way.

```csharp
public (string hello, string world) NamedTupleVersion1()
{
    //...
}

public (string, string) NamedTupleVersion2()
{
    return (hello: "Hello", world: "World");
}
```


#### The difference

The difference is simpler code, less `out` usage and less dummy classes that are only used to transport simple values between methods.

### Advanced scenarios

#### C# 7.0 - Deconstructing tuples (with and without type inference)

When you invoke 3rd party library, tuples will already be with either their name or in a very specific format.

You can deconstruct the tuple and convert it straight variables. How you ask? Easily.

```csharp
var myTuple = (1, "Maxime");

// explicit type definition
(int Age, string Name) = myTuple
// with type inference
var (Age, Name) = myTuple;
Console.WriteLine($"Age: {Age}, Name: {Name}.");
```

If you take the previous example, normally, you would need to access the first property by using `myTuple.Item1`.

Hardly readable. However, we created the `Age` variable easily by deconstructing it. Wherever the tuple come from, you can easily deconstruct it in one line of code with or without type inference.

### Are you going to use it?

When new features are introduced in a language, I like to ask people whether it's a feature they would use.

So please leave me a comment and let me know if it's something that will simplify your life or, at least, your code.
