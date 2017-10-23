---
title: "Part 3 - Advanced mocking functionalities of Moq"
date: 2009-02-22 20:29:00
tags: []
---

See also: [Part 1](/2009/02/part-1-introduction-to-moq.html) - [Part 2](/2009/02/part-2-basic-of-mocking-with-moq.html)

When you have some simple scenario like "when the method "GetTax" is called, return 5$" it's a simple scenario that a lot of mockers will see. However, there is some rarer scenario that people will wonder how to do it.

One of those scenario is with event handlers. The scenario would be like "if a **Product** is added to a **ShoppingCart**, a **ProductAdded** event should be fired".

Let's start with the basic class bellow which implement our scenario:

```cs
namespace MoqSamples
{
    public interface IProduct
    {
        bool IsValid { get; }
    }

    public class ProductEventArgs : EventArgs
    {
        public ProductEventArgs(IProduct product)
        {
            Product = product;
        }

        public IProduct Product { get; private set; }
    }

    public class ShoppingCart
    {
        private readonly List<IProduct> Products = new List<IProduct>();
        public event EventHandler<ProductEventArgs> ProductAdded = delegate { };

        public void Add(IProduct product)
        {
            if (product.IsValid)
            {
                Products.Add(product);
                ProductAdded(this, new ProductEventArgs(product));
            }
        }
    }
}
```

#### Event Handlers

What we want to test here, is every time we add a valid product an event **ProductAdded** should be fired.

I have played with [Moq](http://code.google.com/p/moq/ "Moq") a bit trying to get it to work with ShoppingCart. As I tried to mock the event, I tried to create mocks and use the instructions on Moq site but wasn't able to make it happen. If I tried to mock the class itself it wouldn't allow me to do expectations even if I extracted an interface out of it. If I mock the interface, I lose the logic inside my class. I was thinking about creating a mocked event handlers and see if it ever get called but... you need a mock to create a mocked event handler. With this, we'll have to wait for Moq 3.0 (which is in beta at the moment of writing this article). Here is the test I came up with that didn't work :

```cs
[Test]
public void Adding_A_Valid_Product_Fire_Event()
{
    // Setup our product so that it always returns true on a IsValid verification
    Mock<IProduct> product = new Mock<IProduct>();
    product.Expect(currentProduct => currentProduct.IsValid).Returns(true);

    // setup an event argument for our event
    ProductEventArgs productEventArgs = new ProductEventArgs(product.Object);

    // setup a mocked shopping cart to create our mocked event handler and a true shopping cart to test
    Mock<ShoppingCart> mockedShoppingCart = new Mock<ShoppingCart>();

    //creating the event a mocked event
    MockedEvent<ProductEventArgs> mockedEvent = mockedShoppingCart.CreateEventHandler<ProductEventArgs>();
    mockedShoppingCart.Object.ProductAdded += mockedEvent;
    mockedShoppingCart.Expect(shopping => shopping.Add(product.Object)).Raises(mockedEvent, productEventArgs).Verifiable();

    //making the test
    IShoppingCart myShoppingCart = mockedShoppingCart.Object;
    myShoppingCart.Add(product.Object);

    mockedShoppingCart.Verify();
}
```

And here is my simple fix to test this:

```cs
[Test]
public void Adding_A_Valid_Product_Fire_Event()
{
    // Setup our product so that it always returns true on a IsValid verification
    Mock<IProduct> product = new Mock<IProduct>();
    product.Expect(currentProduct => currentProduct.IsValid).Returns(true);

    // setup an event argument for our event
    ProductEventArgs productEventArgs = new ProductEventArgs(product.Object);

    // creating our objects and events
    ShoppingCart myShoppingCart = new ShoppingCart();
    bool isCalled = false;
    myShoppingCart.ProductAdded += (sender, e) => isCalled = true;

    // Testing the Add method if it fire the event
    myShoppingCart.Add(product.Object);

    // make sure the event was called
    Assert.AreEqual(isCalled, true);
}
```

Way more small and more efficient with the mocking. Sometimes, it's better not to try to bend the framework and find the shortest solution that works.

#### Moq Factories

Moq have factories to help centralize the mocking configuration. The only two configuration available is **CallBase** and **DefaultValue**. Every mock created with the factories will allow you to reuse the configuration and reduce the amount of line for setting up the mock.

Here's a sample for the factory initialization:

```cs
[Test]
public void Moq_Test_With_Factories()
{
    // Initialize factories with default behaviours
    MockFactory mockFactory = new MockFactory(MockBehavior.Default);

    // Setup parameters for mocking
    mockFactory.CallBase = true;
    mockFactory.DefaultValue = DefaultValue.Mock;

    // create mocks with the factory
    Mock<IProduct> product = mockFactory.Create<IProduct>();
}
```

This is of course really easy but... what about the parameters?

##### CallBase

CallBase is defined as "Invoke base class implementation if no expectation overrides the member. This is called "Partial Mock". It allows to mock certain part of a class without having to mock everything.

##### DefaultValue

There is 2 possible values here. One of them is "Empty" which return default value of the class. The one used in the example is "Mock" which allows "automocking". If a property is mockable, a mock is automatically returned.

##### Constructor

The constructor of the MockFactory needs a MockBehaviour parameter. 3 values are possible, Default, Loose and Strict.

Strict mock makes mocked object throw an exception for every call to a mocked object that doesn't have an expectation. Loose (which is also Default) will always return default values or empty arrays or null.

By using the factory properly, it's possible to set one style of mocking and reuse theses settings without having to rewrite 1 or 2 more lines per mock.
