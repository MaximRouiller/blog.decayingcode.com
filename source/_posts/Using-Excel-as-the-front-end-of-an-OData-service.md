---
title: "Using Excel as the front-end of an OData service"
date: 2014-05-23 20:00:00
tags: [tool,wcf,office]
---

### Preparing the project

First we’ll start by creating a WCF Data Service

[![image](/posts/files/6cec115e-0346-4876-9c69-4a401f1c1a3b.png "image")](/posts/files/99d4ac28-92a2-4bc0-bec3-ea119926879f.png)

Once the project is created, we’ll start by deleting Service1.svc and it’s related objects.

We’ll install the following package:
```ps
Install-Package Microsoft.OData.EntityFrameworkProvider -Pre
```

[On Nuget Gallery](https://www.nuget.org/packages/Microsoft.OData.EntityFrameworkProvider/)

Ensure to create an EDMX for your database using EF6\. On my side, I’m using the [Not-Northwind simple database](http://northwindcommunity.codeplex.com/) using all the available tables and none of the views.

Once this is created, we’ll start by creating a brand new service that we’ll call Northwind.svc:

[![image](/posts/files/767f808c-ff0f-410a-a3a1-11f9fa6fdb69.png "image")](/posts/files/c44395fe-fc32-4344-9a0c-0a75524b629d.png)

The class it will generate your will not be compilable right away without injecting our classes.

The class will be called “Northwind” and inherit from DataService<…>.

We’ll replace this:

```cs
public class Northwind : DataService<MySourceClass>
```

By this:

```cs
public class Northwind : EntityFrameworkDataService<NorthwindEntities>
```

This will allow EF6 to connect with WCF Data Services to create a model on your database.

We can test this by hitting F5\. It should return you this:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<service xmlns="http://www.w3.org/2007/app" xmlns:atom="http://www.w3.org/2005/Atom" xml:base="http://localhost:7146/Northwind.svc/">
   <workspace>
      <atom:title>Default</atom:title>
   </workspace>
</service>
```

We see that we don’t have anything exposed. Let’s fix that now shall we? Going back to the code, we can add the following line to Northwind.svc.cs InitializeService method.

```cs
config.SetEntitySetAccessRule("Products", EntitySetRights.AllRead);
```

If we hit F5 again we should see this:

```xml
<?xml version="1.0" encoding="utf-8"?>
<service xml:base="http://localhost:7146/Northwind.svc/" xmlns="http://www.w3.org/2007/app" xmlns:atom="http://www.w3.org/2005/Atom">
  <workspace>
    <atom:title>Default</atom:title>
    <collection href="Products">
      <atom:title>Products</atom:title>
    </collection>
  </workspace>
</service>
```

This means we are ready for hooking this up to Excel.

### Wiring this up to Excel

This is surprisingly the easiest part. First, start Excel 2010 or higher and go to the Data tab and click on the following:

[![image](/posts/files/ba9a8c0b-d86e-4667-abf6-0e6e6c2e1d89.png "image")](/posts/files/6db45a86-b3b5-4d6f-b5ac-a02103e127b4.png)

This will bring up a popup in which we’ll paste our oData URL:

[![image](/posts/files/f6852b52-25d2-42ba-a8ed-88a02a54c3b6.png "image")](/posts/files/10160f15-f925-4167-b977-8650618640c2.png)

Once you press next you will be prompted to select your table (select Products and click Finish).

[![image](/posts/files/4759acf5-d31f-499d-939e-13f9e1371837.png "image")](/posts/files/243702ff-c21e-479c-a0b3-1348bb83e4bd.png)

It will ask you where you want your data and how. 

At that point, it’s up to you to start playing with your data in Excel and expose other endpoint for your business user to start exploiting your data. 