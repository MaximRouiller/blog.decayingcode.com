---
title: "Cross-Cutting Concerns should be handled on ALL projects. No Excuses"
date: 2009-01-16 12:59:00
tags: [architecture]
---

The title say it all. All cross-cutting concerns in a project should be handled or given some thought on **ALL PROJECTS**. No exceptions. No excuses.

Before we go further, what is a cross-cutting concern? Here is the [definition from Wikipedia](http://en.wikipedia.org/wiki/Cross-cutting_concern "Cross-cutting concern"):

> In computer science, cross-cutting concerns are aspects of a program which affect (crosscut) other concerns. These concerns often **cannot be cleanly decomposed** from the rest of the system in both the design and implementation, and **result in either scattering or tangling** of the program, or both.

The perfect example for this is error handling. The error handling is not part of the main model of an application but is required for developers to catch errors and log/display them. Logging is also a cross-cutting concern.

So let's display the 3 most important concerns:

*   Exception Management
*   Logging/Instrumentation
*   Caching

##### Exception Management

This is the most important. It seems like a basic really. Wrapping code in try {...} catch{...} and make sure everything works is the most basic thing to do. I've seen projects without it. Honestly... it's bad. Really bad. Everything was working fine but when something went wrong, nothing could handle it properly.

Adding exception handling to all and every method in an application is not a reasonable thing to do either.

Here is a small checklist for handling exceptions:

1.  Don't catch an exception if there is no relevant information that can be added.
2.  Don't swallow (empty catch) if there is not a good reason to.
3.  Make sure that exception are managed at the contact point between layers so relevant information can be added

Worst of all... you couldn't know wether it was coming from the Presentation, Business or Data layer which leads to horrible debugging.

Which brings us to the next section....

##### Logging / Instrumentation

When an exception thrown, you want to know why. Logging allows you to log everything at a specific location based on the project you are on (database, flat file, WMI, Event Log, etc.). Most people already log a lot of stuff in the code but... is what's logged relevant?

Logging is only important when it's meaningful. Bloating your software with logging won't bring any good. Too much log and nobody will inspect the log for things that went wrong. Too few and the software could generate errors for too long before anybody realize.

I won't go in too much detail but if you want to know [code to logging ratio](http://stackoverflow.com/questions/153524/code-to-logging-ratio) and [the problem with logging](http://www.codinghorror.com/blog/archives/001192.html "The Problem with Logging") there is many information out there.

##### Caching

Caching is too often considered [the root of all evil](http://stackoverflow.com/questions/211414/is-premature-optimization-really-the-root-of-all-evil "Is premature optimization really the root of all evil?"). However, it is evil only if the code become unreadable and it takes a developer 4 hours to get a 1% gain.

I coded a part of an application that generated XML for consumption by a Flash application. I didn't had any specification but I knew that if let that uncached, I would have a bug report the next day on my desk. The caching that I added helped give all flash application responsiveness while keeping the server load under control.

Caching is too often pushed back to a later time and should be considered every time a service or any dynamically generated content is served to the public. The responsiveness will grow while keeping the amount of code to what is necessary.

If you need more argument, please take a look at [ASP.NET Micro Caching: Benefits of a One-Second Cache](http://aspalliance.com/251 "Benefits of a One-Second Cache").

#### How do I do it?

If you haven't been present for the last 10 years or so, I would suggest taking a look at [Enterprise Library](http://www.codeplex.com/entlib "Enterprise Library"). It's a great tool that allows you to handle all those cross-cutting concerns without having to build your own tools.

If you don't want to use Enterprise Library, there is plenty of other framework that will allow to handle those concerns.

Just remember that good coder code, [great coder reuse](http://www.catonmat.net/blog/code-reuse-in-google-chrome-browser/ "Code Reuse in Google Chrome").