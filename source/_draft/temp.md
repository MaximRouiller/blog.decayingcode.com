---
title : "What's new in VS2017 and C# 7.0? Literals"
date: 2017-03-28 09:00
tags: [c#, visual studio, .net]
---


### Literals

Remember when you could do something like this:

```csharp
var hexa = 0x12f4b12a;
```

Well they added binary for those who'd rather write in 0 and 1.

```csharp
var hexa = 0x12f4b12a;
var binary = 0b0110011000111001;
```

While they were there, they also added separators. They do not affect the value in any way and can be applied to any numbers. They are stripped at compile time.

```csharp
var hexa = 0x12f4_b12a;
var binary = 0b0110_0110_0011_1001;
var integer = 1_000_000;
```
