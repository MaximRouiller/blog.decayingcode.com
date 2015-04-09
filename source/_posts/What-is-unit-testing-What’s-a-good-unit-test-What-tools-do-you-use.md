---
title: "What is unit testing? What’s a good unit test? What tools do you use?"
date: 2013-09-06 10:08:00
tags: [mock,unit test]
---

[![mycodecantfail](/posts/files/mycodecantfail_thumb.jpg "mycodecantfail")](/posts/files/mycodecantfail.jpg)

I’ve often encountered projects where tests are not important. We need to deliver yesterday. The first thing that goes out the window when in this kind of rush are the unit tests. The next thing that pokes its head through the window however are bugs. Then as you make more and more modification to the code to reach yesterday’s deadline, functionalities that used to work doesn’t anymore. This is called regression.

When you introduce modifications to a system, there’s no way to ensure that nothing that used to work broke other than by testing it. It might be manual testing where a developer will redo all the steps that could have impacted its code. This kind of testing however take a bit of time to run and relies on the developer to redo all the manual testing every time a modification is done. 

So…

## What is unit testing?

Unit testing is a way to test individual unit of your code. Depending on the type of language you are using, we might talk about modules, classes, functions, procedures or methods as the smallest unit. 

In C#, the smallest unit of work that you have is a method. However, in most scenarios a class will be your unit. 

Unit test will allow you to write in code the expected behaviors of your code. If a module assume a certain behavior coming from your module, changing that behavior could cause a ripple effect of regressions across your system.

## What are the characteristics of a good unit test?

Before showing you some examples, we’ll go over some attributes that makes a unit test a good one.
 > ### Fast
> 
> Unit test must be fast to run. We’re talking milliseconds per test here, not seconds. Since we’ll be creating tests by the numbers, we need them to be fast. Test that are long to run will never be run by any developers. > ### Easy to run
> 
> They should not require the setup of a machine or installing software to run. They should be runnable directly in one easy step. > ### Independent
> 
> They should not require a specific run order. Tests should be runnable in parallel, in sequence or in any order that the test runner will decide. This ensure the next attribute. > ### Repeatable
> 
> They should always yield the same result independent of the actual machine. 

All those characteristics throws off any test that requires a database, network access or access to specific files on a disk. All external resources could potentially be locked, not accessible or long to initialize. 

Another attribute that might considered such is **Automatic**. Having tests run as part of the build process will ensure that no commits will break any previously tested functionalities. 

## What are the benefits of unit testing?

### Fail fast

Having tests that fails as soon as something doesn’t behave as expected will allow you to easily find problems with your code. 

### Easier refactoring

So you’ve refactored your code and its all cleaner now. Does all the tests passes? If yes, missing accomplished. Otherwise, you’ve introduced regression in your system. Tests on a unit you are refactoring will provide you the peace of mind that whatever you did, it still works. You will find yourself refactoring much more often as the confidence in your code (and tests) will increase.

### Code documentation

If your tests are named properly, they will indicate what kind of behaviors you are testing. This will allow other developers to know exactly what they broke when one of them failed. However, it provides great documentation for developers that just joined your team and is wondering how to use your class. With a whole set of test, they can just see what kind of behavior that class has.

### Design

If you do TDD or any other type of Test First code, tests will help you design your class in the way the user of the class is meant to use it. This might sound funny but by starting by _**How to use it **_rather than the implementation, it will lead you to simpler code and easier to setup.

## Techniques that will help you test

### Code against an abstraction. Not an implementation.

Coding against an interface or an abstract class might sound like one too many file in your project. However, this will provide you a seam to inject dependencies. You have an MailService that sends email? Try coding against it’s interface (IMailService) instead. When you need to create that dependency, you can use a tool like [NSubstitute](/post/Introduction-to-NSubstitute "Introduction to NSubstitute") to create a mock for you or you can just create one manually. The same can be done for databases, network resources like web services. 

Remember that you don’t want to test if your SMTP server can do its job. We assume it can. If you want to validate things like this, they are to be done in a separate test project under the label “Integration Tests” or “System Tests”. They are by definition slow to run and won’t, and don’t need, to be run as often. Those tests can be run in a nightly build and it’s often enough.

### Name your tests properly

By following some conventions, your tests can be easy to understand. You have to name your test so that you can understand them when you come back 6 months later while inebriated.

Personally, I like to use the following format: **WHEN** I do this I **EXPECT** something **TO RETURN/DO** another thing. This normally allows you to describe properly behaviors of a method or a class. This is only a recommendation. If you have a better convention, use it. As long as I know what that test is doing without looking at the code, it’s fine. 

### One act and one assert per test

Your test should only invoke the class/method you are testing only once and assert the result of only one thing. What we are looking for is “Only one reason to fail”. If you remove one line of code it should break one test (or as few as possible). Imagine the scenario where you introduce a new line of code to introduce Feature A and 200 tests fails. You will end-up digging in the code asking yourself what you broke. Keep things simple for you and the next guy.

### Follow the triple A (AAA)

Its called Arrange/Act/Assert. It’s also known in BDD as Given/When/Then. Arrange will contain your necessary code that is required to test your class. Act will invoke the method that is currently being tested. Assert will validate the what was done in the act. I try to split my test methods with three comments with the same name as the AAA to help future developers understand what the hell I was doing and where.

### Write a test for every bug you find

Whenever a bug is reported, I write a test that reproduce the bug (that test is supposed to fail). Then, I’ll write the necessary code to make that test turn green. If you do that for every bug you find, you will end also do regression testing on your past failures.

## Limits of unit testing

Unit testing is there to test small units. It’s in no way a complete test of your system. If you database or your mailing system is down, unit testing will not help you detect those problems. Different kind of test (much slower) are needed for those. I’ve put a lot of emphasis in this article about unit testing. It doesn’t mean that integration testing shouldn’t be done. It just means that you should have way more unit test than integration tests. 

Unit testing will also only test what you have. It will not do exploratory testing to find unexpected behaviors. I recommend having your developers or (if you are lucky to have them) QA team check your system in its entirety before every release to ensure that nothing is broken. 

## Tools used for unit testing

If you need to do mocking with a tool, I recommend [NSubstitute](/post/Introduction-to-NSubstitute "Introduction to NSubstitute").

If you need a unit test framework, I would either use [MSTest](http://en.wikipedia.org/wiki/Visual_Studio_Unit_Testing_Framework) or [NUnit](http://nunit.org/). 

If you want to shorten the time between when tests are run, I recommend [NCrunch](http://www.ncrunch.net/). As you modify your code, it compiles and runs impacted tests in real time.

If you want to see your test coverage, I recommend [dotCover](http://www.jetbrains.com/dotcover/) by the same people who does [ReSharper](http://www.jetbrains.com/resharper/index.html). 

If you want to see your coverage in a higher level, I recommend [NDepend](http://ndepend.com/). It will incorporate metrics that are essential to proper testing coverage. 