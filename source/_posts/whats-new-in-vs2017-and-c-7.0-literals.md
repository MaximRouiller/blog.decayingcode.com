---
title : "What's new in VS2017 and C# 7.0? - Literals"
date: 2017-03-28 09:00
tags: [c#, visual studio, .net]
---


### Literals

#### C# 6.0

The C# 6.0 way to define integers in .NET is to just type the number and there's no magic about it.

You can assign it directly by typing the number or if you have a specific hexadecimal, you can use the literal annotation `0x` to define it.

Not teaching anyone anything new today with the following piece of code.

```csharp
int hexa = 0x12f4b12a;
int i = 1235;
```

#### C# 7.0

Now in C# 7.0, there's support for binary annotations. If you have a specific binary representation that you want to test, you can use the literal annotation `0b` to define it.

```csharp
var binary = 0b0110011000111001;
```

Another nice feature that is fun to use is the separator. It was supposed to be introduced in C# 6.0 but was delayed to 7.0.

They do not affect the value in any way and can be applied to any literals.

```csharp
var hexa = 0x12f4_b12a;
var binary = 0b0110_0110_0011_1001;
var integer = 1_000_000;
```

They can be applied any where and will not impact the evaluation of the number.

### Are you going to use it?

When new features are introduced in a language, I like to ask people whether it's a feature they would use.

So please leave me a comment and let me know if binary or separators are a feature that will be used.
