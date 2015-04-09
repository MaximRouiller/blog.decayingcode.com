---
title: "Hidden bug: Parameter passing"
date: 2013-05-07 06:17:13
tags: [c#]
---

There is a bug in the following piece of code. The bug could potentially break behaviours. 

```cs
private int ReturnDefaultYear(int? year)
{
    if (year < 1970) // LowerYearBound defined as a int elsewhere
        year = null;

    var result = year ?? 1970;
    return result;
}
```

Found it yet? I’ll give you a hint. When passing parameters in .NET, value types are copied (int, double, float, etc.) but reference type (string, nullable, etc.) have their reference passed around. So when you change them, their value is not lost when leaving the method scope. They are persisted up to the highest level.

In this scenario, the parameter that was passed for “year” would become null.

Try running this simple code in a Console Application and you will understand the problem:


```cs
class Program
{
    static void Main(string[] args)
    {
        int? test = 1;
        Console.WriteLine(ConvertTest(test));
        Console.WriteLine(ConvertTest(test++));
        Console.WriteLine(ConvertTest(test++));
        Console.WriteLine(ConvertTest(test++));

        Console.ReadLine();
    }

    public static int ConvertTest(int? test)
    {
        if(test > 1)
            test = null;
        return test ?? 0;
    }
}
```