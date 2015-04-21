---
title: "Do modern compilers optimize the x * 2 operation to x << 1?"
date: 2009-02-17 21:01:00
tags: [optimization]
---

I was wondering wether the C++ compiler inside Visual Studio 2008 was optimizing it the way it would logically be. So I asked the question on [Stackoverflow](http://stackoverflow.com/questions/235072/do-modern-compilers-optimize-the-x-2-operation-to-x-1). It was among my first questions and was to see how the community would answer (yeah I'm lazy).

I was promptly answered by [Rob Walker](http://stackoverflow.com/users/3631/rob-walker). He showed me what the compiler outputted.

32bit:

```cs
01391000  push        ecx
int x = 0;

scanf("%d", &amp;x);
01391001  lea         eax,[esp]
01391004  push        eax
01391005  push        offset string "%d" (13920F4h)
0139100A  mov         dword ptr [esp+8],0
01391012  call        dword ptr [__imp__scanf (13920A4h)]

int y = x * 2;
01391018  mov         ecx,dword ptr [esp+8]
0139101C  lea         edx,[ecx+ecx]
```

64bit:

```cs
int y = x * 2;
000000013FB9101E  mov         edx,dword ptr [x]

printf("%d", y);
000000013FB91022  lea         rcx,[string "%d" (13FB921B0h)]
000000013FB91029  add         edx,edx
```

While this gives us a lot of information about what it does, it's normally a pretty bad idea to try to outsmart the compiler. The compiler have it's own way to optimize some operations to make sure that it run best on the targeted platform.

To quote [Raymond Chen](http://blogs.msdn.com/oldnewthing/archive/2005/05/27/422551.aspx):

> Of course, the compiler is free to recognize this and rewrite your multiplication or shift operation. In fact, it is very likely to do this, because x+x is more easily pairable than a multiplication or shift. Your shift or multiply-by-two is probably going to be rewritten as something closer to an add eax, eax instruction.
> 
> The moral of the story is to **write what you mean**. If you want to divide by two, then write "/2", not "&gt;&gt;1"

Bold was added by me for emphasis. Raymond Chen wrote that in 2005\. Write what you mean. Wow. If you want to multiply then write "*2" and if you want to divide write "/". If you are meaning to shift bits, then so! But don't try to outsmart the compiler by doing bit shifting.

If you are doing bit shifting do gain performance, **you are doing something wrong**.&Acirc;&nbsp; There is many other way to gain performance in the kind of application we, developers, are building and bit shifting is not one of them.

You are simply losing readability for _maybe_ a few milliseconds.
