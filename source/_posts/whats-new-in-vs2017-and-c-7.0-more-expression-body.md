---
title : "What's new in VS2017 and C# 7.0? - More Expression Body"
date: 2017-03-31 09:01:00
tags: [c#, visual studio, .net]
---

Expression Bodied functions is a relatively new concept brought to use in C# 6.0 to facilitate the creation of properties that included too much ceremony.

C# 7.0 remove even more ceremony from many more concepts.

#### C# 6.0

Previously, C# 6.0 introduced the concept of Expression bodies functions.

Here are a few examples.

```csharp
// get only expression bodied property
public string MyProperty => "Some value";
// expression bodies method
public string MyMethod(string a, string b) => return a + b;
```

#### C# 7.0

With the new release of C# 7.0, the concept has been added to:
* constructors
* destructors
* getters
* setters

Here are a few examples.

```csharp
class TestClass
{
    private string _name;

    // expression-bodied constructor
    public TestClass(name) => _name = name;
    // expression-bodied destructor
    ~TestClass() => _name = null;

    public string Name
    {
        get => _name;
        set => _name = value;
    }
}
```



#### The difference

The main difference in your code will be the amount of line of codes that will be used for useless curly braces or plumbing code.

Yet again, this new version of C# offer you more ways to keep your code concise and easier to read.


### Are you going to use it?

I know that not everyone will necessarily use all of these new form of expression body. But I'm interested in your opinion.

So please leave me a comment and let me know if it's something that will simplify your life or, at least, your code.
