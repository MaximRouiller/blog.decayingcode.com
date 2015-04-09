---
title: "Creating a Self-Hosted OWIN Application"
date: 2014-05-29 15:04:21
tags: [owin]
---

So I know there’s a lot of demo that cover this already but this is normally part of my demo on OWIN/Katana and I always forget which package to install so… you dear reader are going to see my reminder on how to do it.

### Creating a new console application

The first step is to create a new Console application. I’m not going to include screenshots of that since I’m pretty sure that you are all smart enough to not need a how-to on creating console app.

### Startup.cs

Every OWIN app need a Startup.cs file. If you don’t have [SideWaffle](http://www.sidewaffle.com) installed, you’ll need to copy/paste my code but why do that when you can have a template for it? Here’s my barebone one.

```cs
using Microsoft.Owin;
using Owin;

[assembly: OwinStartup(typeof(MySelfHostedApplication.Startup))]

namespace MySelfHostedApplication
{
    public class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            app.UseWelcomePage();
        }
    }
}
```

The only thing that won’t work yet for anyone is the “UseWelcomePage”. We’re fixing that right now.

In the Package Manager Console run the following command:
```ps
Install-Package Microsoft.Owin.SelfHost
```

There. We should now have something that compiles! Not working yet but compiling. Baby step.

### Starting the server itself

Since it’s a console application, we only have an empty Main() that does nothing and adding Startup.cs will not do any magic. We still need to wire-up things up neatly before going any further. Ready?

```cs
using System;
using Microsoft.Owin.Hosting;

namespace MySelfHostedApplication
{
    class Program
    {
        static void Main(string[] args)
        {
            using (WebApp.Start&lt;Startup&gt;("http://localhost:8888"))
            {
                Console.WriteLine("Server started... press ENTER to shut down");
                Console.ReadLine();
            }
        }
    }
}
```

That’s it. You could remove the WriteLine but I like to have something in my window instead of a blank screen. The ReadLine is required or the server is going to be disposed. 

### Testing it

Now press F5 and open your browser to [http://localhost:8888](http://localhost:8888) and you should see a nice welcome page.

Enjoy!