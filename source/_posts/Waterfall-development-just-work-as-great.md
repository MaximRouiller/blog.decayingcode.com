---
title: "Waterfall development just work as great"
date: 2009-03-04 01:39:00
tags: []
---

Waterfall development is still a valid way to develop software. Setting up the requirements, making proper analysis, coding and then testing works just as fine. However... not for ever changing software like a website.

If I were to build an e-Commerce website, I would never choose to go Waterfall. I would love to go SCRUM or XP. Agile development have the advantage of including the client inside the development process. It allows the client to change his mind on some things that seemed good at first but that were finally a bad idea. There is some ideas that can only be rated as "bad" when you are working to develop them. Of course, if some features take longer, it's easier to find out quickly that the project is going to be late and that some features are going to be left out.

Agile development as worked well so far in custom application development, websites, e-Commerce, product development, etc. However, as good as Agile might be... there is one strong point where I think the waterfall approach is still relevant.

Some software need to be bug free and have detailed specifications about the features. What are those software? One example is what a group of people inside Lockheed Martin is developing for the Space Shuttle which [FastCompany talks about](http://www.fastcompany.com/magazine/06/writestuff.html). This software need to be bug free. Of course, it needs to be thoroughly tested and must pass the strongest inspections. Every changes to the specifications must be approved by multiple persons and any change inside the code base without a valid reason is not allowed. They do not do agile. They do waterfall. What about the bugs?

> This software never crashes. It never needs to be re-booted. This software is bug-free. It is perfect, as perfect as human beings have achieved. Consider these stats : the last three versions of the program -- each 420,000 lines long-had just one error each. The last 11 versions of this software had a total of 17 errors. Commercial programs of equivalent complexity would have 5,000 errors.

So how come Waterfall works in this case? Of course, because the software is based on pieces of hardware that rarely need to change, it reduce the amount of compatibility problem. The software doesn't need to work on 100 types of space shuttle. It has only one physical requirement. It also works because once specifications are written, they are to be followed at all cost. Changes are expansive and must be approved every time. The software is also never updated while in use. Of course you'll never see this in an e-Commerce website!

So let's resume:

*   Hardware that rarely change
*   Precise specifications
*   Expansive change
*   Once deployed, can't be changed

Does that bring other examples? The simple things I could think would be any piece of hardware that have software inside. Let's go with the Microwave. Your microwave have a software inside. Let's see how many points it meet shall we? First, the microwave hardware will NEVER change. Nobody is pimping out his microwave so we can assume that the hardware stays the same for all it's usable life. The specification for a microwave software rarely change (timer, defrost settings, power levels, etc.). If there is a change that must be made, it's probably because of a hardware change which is expansive. And finally, no microwave is Wi-Fi enabled or have a USB connection to update it's firmware.

We can safely assume that the Waterfall model must have been among the first software development process to be used. People first programmed chips, board, "simple" OS or OS with limited distribution. Of course, back then the formula worked great because of exactly the same four points I mentioned. The model started to break when building software for computers that varied largely in configuration (RAM, CPU, etc.). The model tried to be used but suddenly, development time sky-rocketed through the roof. A new model was seen as necessary.

So please, unless you are reprogramming your microwave for some evil plans, don't use Waterfall. The main weakness of waterfall was the lack of user inputs. Even the Sashimi model is not enough. We need rapid feedback and constant testing. We are not developing perfect software that must never fails. But make sure it does before hitting the client.

