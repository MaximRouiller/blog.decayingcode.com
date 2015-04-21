---
title: "Why should I use mocking objects in my Unit Test?"
date: 2009-02-18 20:13:00
tags: []
---

If we cut out any "fanboy" or favouritism toward certain framework and that we try to keep it in a one liner... I would say: "To simulate behaviours of objects that are impractical or impossible to incorporate inside a unit test".

The Wikipedia's article about [Mock Object](http://en.wikipedia.org/wiki/Mock_Object) mention some reason an object should be mocked.

#### The object...

##### Supplies Non-Deterministic Results

By "non-deterministic" we mean everything from time, currency rate, shipping rate, etc. Any value that could be changing because of a specific implementation such as algorithm should be mocked. Mocked object allows you to return predetermined value that are independent of the algorithm/time/etc.

This allows to more easily test the state of the System Under Test (SUT) after running some methods.

##### Has States that are difficult to create or reproduce

The example given by Wikipedia is "network error". It's difficult to reproduce this kind of situation on every developers station. Other situation might include security, location of the test on disk, network availability (not just errors). If some objects that the SUT is using require any of those, the tests WILL fail somewhere and somehow. If it's not on a developer's machine, it's going to be on the build machine.

Mocking those objects and giving them proper behaviour will remove any required "settings" that are necessary to run a Unit Test.

##### Is Slow

Databases, network, file access (up to a point) are all slow. If your SUT is an ObjectService that is using a Repository and you are hitting directly on the databases, it is bound to be slow. Of course the database can cope with it. But as you had more tests, the unit will soon take HOURS to run. With a small in-memory database will save the day and run those tests in less than a few minutes.

A mocked repository might just keep a collection of saved object so that when a "Get" method is called, it's readily available in this collection. This kind of mock is called a "fake" in the world of Mocking. It implements more complex behaviour but allows for easy initialization and more timely responses.

##### Does not yet exist or may change behaviour

If the only thing that is currently available within your system boundaries are contracts (Interfaces in C#), it's more easy to mock the interface that the SUT is requiring and go with this temporarily while the component is being developed. This allow testing and coding at the same time.

#### Conclusion

Mocking is an excellent tool to test a specific object under controlled conditions. Of course, those conditions are bound to change and tests are going to be maintained. Why I particularly like is when I use a mocking framework, I don't need to create 1000+ objects (exaggerating here) with some specific behaviours or create "too intelligent" mock that will have to be maintained. I dynamically declare a mock with [my favourite mocking framework](http://code.google.com/p/moq/ "Moq") with the expected call and the expected returns and I go through with that.

What normally happens is I have considerably less mocking objects inside my Unit Tests and the only objects that are left standing are some in-memory database objects with simple implementation that would be to hard to define with a mocking framework.

