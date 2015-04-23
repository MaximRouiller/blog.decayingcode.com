---
title: "Implementing a Chain-of-responsibility or \"Pipeline\" in C#"
date: 2009-03-05 00:00:00
tags: [architecture]
---

Anti-Patterns are interesting in showing you what you are doing wrong. However, patterns are also interesting in showing you **how to do it well**.

This time, I want to show how to implement a simple [Chain-of-responsibility](http://en.wikipedia.org/wiki/Chain_of_responsibility_pattern) pattern. Our example is going to be based on a simple e-Commerce data model.

#### The Domain Model

**Product** which will have some basic attributes like a price, a name and a collection of applied discounts.

**Discount** which is going to be an actual discount implementation. More class are going to be derived from this base class.

That is all we are going to need for this pattern. However, it would be smart to have a class that would assign discounts to product based on certain rules.

Let's start by writing our **Product** class and our **Discount** interface:

```csharp
public class Product
{
    private readonly List<IDiscount> _appliedDiscount = new List<IDiscount>();

    public string ProductName { get; private set; }
    public decimal OriginalPrice { get; private set; }

    public decimal DiscountedPrice
    {
        get
        {
            decimal discountedPrice = OriginalPrice;
            return discountedPrice;
        }
    }

    public Product(string productName, decimal productPrice)
    {
        ProductName = productName;
        OriginalPrice = productPrice;
    }

    public List<IDiscount> AppliedDiscount
    {
        get
        {
            return _appliedDiscount;
        }
    }
}

public interface IDiscount
{
    decimal ApplyDiscount(decimal productPrice);
}
```

Right now, the "DiscountedPrice" is simply returning our "OriginalPrice". Let's implement the proper discount commands:

```csharp
public decimal DiscountedPrice
{
    get
    {
        decimal discountedPrice = OriginalPrice;
        
        foreach (IDiscount discount in _appliedDiscount)
        discountedPrice = discount.ApplyDiscount(discountedPrice);
        
        return discountedPrice;
    }
}
```

Now that we have an algorithm that will apply all discounts, let's create a few **Discount** class:

```csharp
public class PercentageDiscount : IDiscount
{
  public decimal PercentDiscount { get; set; }

  public PercentageDiscount(decimal percentDiscount)
  {
    PercentDiscount = percentDiscount;
  }

  public decimal ApplyDiscount(decimal productPrice)
  {
    return productPrice - (productPrice*PercentDiscount);
  }
}

public class FixPriceDiscount : IDiscount
{
  public decimal PriceDiscount { get; set; }

  public FixPriceDiscount(decimal priceDiscount)
  {
    PriceDiscount = priceDiscount;
  }

  public decimal ApplyDiscount(decimal productPrice)
  {
    return productPrice - PriceDiscount;
  }
}
```

So now we have a class that implement a percentage discount and another one that impose a fixed rate discount. Of course, our current implementation should **NEVER** be used in a real system as it is now. Validations must be done for a positive price and maybe some extra verification that we are not underselling the item.

Let's use this current implementation:

```csharp
// Creating a product worth 50$
Product currentProduct = new Product("Simple product", 50.0M);

Console.WriteLine(string.Format("Original Price: {0}", currentProduct.OriginalPrice));

// Give a 10% rebate on the product
currentProduct.AppliedDiscount.Add(new PercentageDiscount(0.1M));
Console.WriteLine(string.Format("Discounted Price: {0}", currentProduct.DiscountedPrice));

//Give an extra 10$ off on the product
currentProduct.AppliedDiscount.Add(new FixPriceDiscount(10.0M));
Console.WriteLine(string.Format("Discounted Price: {0}", currentProduct.DiscountedPrice));
```

This will output in order 45.00$ and 35.00$.  It's important to be aware that the discount interfaces are not aware that they are being applied to a product. They could be reused in any other model that accepts an **IDiscount**.

#### Conclusion

By chaining Strategy Pattern (the discount algorithms), we can increase the amount of flexibility inside our model and increase the reuse of common algorithms. It would also be easy with a simple rule engine to apply discounts to product that match certain rules.

Other uses of a chain-of-responsibility could be when dealing with objects that could have multiple rules applied to them based on different conditions. The conditions would then be moved from the object itself to a "Command" and then reused exactly the same way we did here.
