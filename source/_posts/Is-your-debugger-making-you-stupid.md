---
title: "Is your debugger making you stupid?"
date: 2009-06-29 20:37:00
tags: []
---

What is one of the greatest advance of Visual Studio since the coming of .NET? You might think it is the Garbage Collector or the IL which allows interoperability between languages? I think of one the great advance of Visual Studio 2003 (all the way through Visual Studio 2010) is the debugger. Previously, debugger were hardly as powerful as Visual Studio. And that is the problem.

#### What is debugger?

The quote from Wikipedia is "**a debugger is a computer program that is used to test and debug other programs**". The debugger is used to find bugs and figure out how to fix them.

The debugger is used to go step by step, step backward, step-into, etc. All this, in the hope of reproducing a bug.

#### Why does it make me stupid?

It might be one of the most powerful tools that you have under your hand. But it's also one of the most dangerous. It encourages you to test your software yourself without having tools do the job for you. Once you are done debugging a module, you will never debug it again unless there is a bug in it. Then you start building around this module and you test the new modules against the first module. And then starts the fun. You start modifying the first module but don't test again the first scenario that you built. Now, the next time another developer build something based on the same module, there is two thing that can happen. The first is that the developer is going to be afraid to change the module and will duplicate the code in another module to make sure he doesn't break anything. The second option is that the developer is&nbsp; going to change the module anyway and rerun the application to make sure he didn't break anything obvious. Okay, there is a third option that consist in adding test to address the new behaviour but we're not interested in the good stuff. Just the bad.

#### Ripples of the Debugger

Okay. After I just told you this nice little story, what do you think will happen in the future? As the other developers goes into the code, they will add over the modules again and again. As the modifications keep on coming, the module will keep on changing. As the changes goes "debugger tested" only, bugs will start to appear in modules that never had bugs before. To "test" the right behaviour, the team will start to add test script to execute manually to make sure no bugs are left behind. This will require interns or QA people to run the test.

#### The solution?

Infect your code with test and stop using the debugger. That's simple. I know that Ruby doesn't have a debugger integrated inside the main UI it's using. Ruby developer still manage to deliver quality code without the integrated debugger. In fact, lots of developer manage to make great software without debugger. Running without debugger and test however is NOT the solution. You must ensure that your code is covered with code as much as possible. When you find a bug, make a test that reproduce the bug and fix the production code. As your code gets tested, additional modules will not make break unless they break a test. This is the solution. This is the way to make good and clean code.