---
title: "Software Developer Computer Minimum Requirements October 2014"
date: 2014-10-16 20:44:13
tags: [hardware]
---

I know that [Scott Hanselman](http://www.hanselman.com/) and [Jeff Atwood](http://blog.codinghorror.com/) have already done something similar.

Today, I'm bringing you the minimum specs that are required to do software development on a Windows Machine.

P.S.: If you are building your own desktop, I recommend [PCPartPicker](http://pcpartpicker.com/).

### Processor

#### Recommendation

<span style="font-weight: 700;">Intel:</span> Intel Core i7-4790K

<span style="font-weight: 700;">AMD:</span> AMD FX-9590

Unless you use a lot of software that supports multi-threading, a simple 4 core here will work out for most needs.

### Memory

#### Recommendation

Minimum 8GB. 16GB is better.

My minimum requirement here is 8GB. I run a database engine and Visual Studio. SQL Server can easily take 2Gb with some big queries. If you have extensions installed for Visual Studio, it will quickly raise to 1GB of usage per instance and finally... Chrome. With multiple extensions and multiple pages running... you will quickly reach 4GB.

So get 8GB as the bare minimum. If you are running Virtual Machines, get 16GB. It won't be too much. There's no such thing as too much RAM when doing software development.

### Hard-drive

#### Recommendation

512 GB SSD drive

I can't recommend enough an SSD. Most tools that you use on a development machine will require a lot of I/O. Especially random read. When a compiler starts and retrieve all your source code to compile, it will need to read from all those file. Same thing if you have tooling like ReSharper or CodeRush. I/O speed is crucial. This requirement is even more important on a laptop. Traditionally, PC maker put a 5200RPM HDD on a laptop to reduce power usage. However, 5200 RPM while doing development will be felt everywhere.

Get an SSD.

If you need bigger storage (terabytes), you can always get a second hard-drive of the HDD type instead. Slower but capacities are also higher. On most laptop, you will need external storage for this hard drive so make sure it is USB3 compatible.

### Graphic Card

Unless you do graphic rendering or are working with graphic tools that require a beast of a card... this is where you will put the less amount of money.

Make sure to get enough of them for your amount of monitors and that they can provide the right resolution/refresh rate.

### Monitors

My minimum requirement nowadays is 22 inches. 4K is nice but is not part of the "minimum" requirement. I enjoy a 1920x1080 resolution. If you are buying them for someone else, make sure they can be rotated. Some developers like to have a vertical screen when reading code.

### To Laptop or not to Laptop

Some company go Laptop for everyone. Personally, if the development machine never need to be taken out of the building, you can go desktop. You will save a bit on all the required accessories (docking port, wireless mouse, extra charger, etc.).

My personal scenario takes me to clients all over the city as well as doing presentations left and right. Laptop it is for me.