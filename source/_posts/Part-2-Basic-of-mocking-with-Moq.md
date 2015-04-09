---
title: "Part 2 - Basic of mocking with Moq"
date: 2009-02-11 22:22:00
tags: []
---

See also: &Acirc;&nbsp;[Part 1](http://blog.decayingcode.com/2009/02/part-1-introduction-to-moq.html)&Acirc;&nbsp;- [Part 3](http://blog.decayingcode.com/2009/02/part-3-advanced-mocking-functionalities.html)

As every mocking framework, except TypeMock which can perform differently, every mocked class can't be sealed and methods that need to be mocked need to be public. If&Acirc;&nbsp; the class is not inheriting from an interface, the method that are being mocked need to be virtual.

Once this is cleared... let's show a simple example of a **Product** having it's price calculated with a **Tax Calculator**.

Here's what we are starting with:

```cs
public class Product
{
    public int ID { get; set; }
    public String Name { get; set; }
    public decimal RawPrice { get; set; }
    public decimal GetPriceWithTax(ITaxCalculator calculator)
    {
        return calculator.GetTax(RawPrice) + RawPrice;
    }
}

public interface ITaxCalculator
{
  decimal GetTax(decimal rawPrice);
}
```

The method we want to test here is **Product.GetPriceWithTax(ITaxCalculator)**. At the same time, we don't want to instantiate a real tax calculator which gets it's data from a configuration or a database. Unit tests should never depend upon your application's configuration or a database. By "application's configuration", I mean "App.config" or "web.config" which are often changed during the life of an application and might inadvertently fail your tests.

So, we are going to simply mock our tax calculator like this:

```cs
//Initialize our product
Product myProduct = new Product {ID = 1, Name = "Simple Product", RawPrice = 25.0M};

//Create a mock with Moq
Mock<ITaxCalculator> fakeTaxCalculator = new Mock<ITaxCalculator>();

// make sure to return 5$ of tax for a 25$ product
fakeTaxCalculator.Expect(tax => tax.GetTax(25.0M)).Returns(5.0M);
```

Now It all depends on what you want to&Acirc;&nbsp; test. Depending if you are a "State" (Classic) or "Behaviour verification" (Mockist), you will want to test different things. If you don't know the difference, don't bother now but you might want to look at [this article by Martin Fowler](http://martinfowler.com/articles/mocksArentStubs.html#ClassicalAndMockistTesting "Mocks Aren").

So if we want to make sure that "GetTax" from our interface was called:

```cs
// Retrived the calculated tax
decimal calculatedTax = myProduct.GetPriceWithTax(fakeTaxCalculator.Object);

// Verify that the "GetTax" method was called from  the interface
fakeTaxCalculator.Verify(tax => tax.GetTax(25.0M));
```

If you want to make sure that the calculated price equal your product price with your tax added (which confirm that the taxes were calculated):

```cs
// Retrieved the calculated tax
decimal calculatedTax = myProduct.GetPriceWithTax(fakeTaxCalculator.Object);

// Make sure that the taxes were calculated
Assert.AreEqual(calculatedTax, 30.0M);
```

What's the difference? The first example verify the behaviour by making sure that "GetTax" was called. It doesn't care about the value returned. It could return 100$ and it would care. All that mattered in this example was that GetTax was called. Once this is done, we can assume that the expected behaviour was confirmed.

The second example is a state verification. We throw 25$ inside the tax calculator and we expect the tax calculator to return 5$ for a total price of 30$. It wouldn't call GetTax and it wouldn't care. As long as the proper value is returned, it's valid.

Some people will argue that behaviour is better than state (or vice versa). Personally, I'm a fan of both. A good example is that I might want to verify that an invalid invoice will not be persisted to the database and a behaviour verification approach is perfect for this case. But if I'm verifying (like in this case) that the tax were properly calculated, state behaviour is more often than not quicker and more easier to understand.

Nothing prevent your from doing both and making sure that everything works. I'm still not a full fledged TDD developer but I'm trying as much as possible to make tests for my classes as often as possible.

If you found this article helpful, please leave a comment! They will be mostly helpful for my presentation on February 25th 2009 at [www.dotnetmontreal.com](http://www.dotnetmontreal.com/).
