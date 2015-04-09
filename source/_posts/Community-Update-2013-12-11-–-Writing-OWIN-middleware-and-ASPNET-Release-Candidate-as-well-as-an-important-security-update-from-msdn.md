---
title: "Community Update (2013/12/11) – Writing #OWIN middleware and #ASPNET Release Candidate as well as an important security update from #msdn"
date: 2013-12-11 13:53:42
tags: [community update]
---

Not too much content but some very important one. Let’s start with the fun stuff first.

We have a nice tutorial on how to write OWIN middleware by OdeToCode. Then we have the OWIN ASP.NET Identity package for Entity Framework.&nbsp; Also in the news, the official blog post from MSDN about the latest Release Candidate of ASP.NET MVC 5.1, Web API 2.1 and Web Pages 3.1\. 

An oldie but a goodie, how to change where the ResourceManager get its strings. The last article got you covered.

Finally the most most important one is the ASP.NET December 2013 security updates. This is a MUST READ. One very important note that I will copy/paste is that EnableViewStateMac is going to go deprecated in future version of the .NET Framework. If you are relying on it for your websites to work, you must take time to fix it as soon as possible. No time to snooze on this one. 
 > **Important note:**
> The next version of ASP.NET will forbid setting EnableViewStateMac=false. Applications which set EnableViewStateMac=false may no longer function properly once this update is pushed out. Web developers must take this time to ensure that their applications do not set this switch to an insecure value. 

### OWIN

[Writing OWIN Middleware (feedly.com)](http://feedly.com/e/rQCCYavg)

[NuGet Gallery | Microsoft ASP.NET Identity Owin 1.0.0 (www.nuget.org)](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)

### ASP.NET

[ASP.NET December 2013 Security Updates (blogs.msdn.com)](http://blogs.msdn.com/b/webdev/archive/2013/12/10/asp-net-december-2013-security-updates.aspx)

[Release Candidates for ASP.NET MVC 5.1, Web API 2.1 and Web Page 3.1\. (blogs.msdn.com)](http://blogs.msdn.com/b/webdev/archive/2013/12/09/asp-net-and-web-tools-2013-2-preview-for-visual-studio-2013.aspx)

[ASP.NET MVC 5 Internationalization · How to Store Strings in a Database or Xml (feedly.com)](http://feedly.com/e/mGalPbXq)