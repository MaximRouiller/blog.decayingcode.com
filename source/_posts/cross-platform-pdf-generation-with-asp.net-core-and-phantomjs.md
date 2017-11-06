---
title: "Cross Platform PDF generation with ASP.NET Core and PhantomJS"
date: 2016-07-14 10:00:00
tags: [asp.net core, tools]
---

That post will be short but it's worth it. One of the easiest way I found to statically generate reports from .NET Core is to simply not to use ASP.NET Core for rendering PDF.

At the current state of affair, most libraries aren't up to date and may take a bit of time before they are ready to go. So how do you generate PDF without involving other Nuget packages?

![PhantomJS](/posts/files/phantomjs-logo.png)

### What to render with ASP.NET Core?

Let's take an example like invoices. I could create an internal-only MVC website that have only one unsecure controller that renders invoices in HMTL on specific URLs like `/Invoice/INV000001`.

Once you can render one invoice per page, you have 99% of the work done. What remains is to generate the PDF from that HTML.

### How does it work with PhantomJS

By using scripts like [rasterize.js](https://github.com/ariya/phantomjs/blob/master/examples/rasterize.js) to interface with [phantomjs][phantomjs], you can easily create PDF files in no time.

```none
phantomjs rasterize.js http://localhost:5000/Invoice/INV000001 INV000001.pdf
```

And that's it. You now have a PDF. The only thing left is to generate the list of URLs and associated filenames for the invoices you want to generate and run that list against that script.

It could even be part of an generation flow where a message could be put on a queue to generate those invoice asynchronously.

```json
{
    'invoiceId': 'INV000001',
    'filename': 'INV000001.pdf'
}
```

From there, we could have a swarm of processes that just runs the phantomjs script and generate the proper invoice at the proper destination like a blob storage.

### Cross-Platform concerns

The best part about this process is that PhantomJS is available on Linux, OSX as well as Windows. With ASP.NET Core also being available on all these platforms, you currently have a cross-platform solution that will meet most of your requirements in term of cross-platform needs.

Even better, this scenario works very well on Azure. By opting for an asynchronous flow, we allow our operation to scale better and allow ourselves to slice our operations to a more maintainable size.

If you have opinion on the matter, please leave a comment!

[phantomjs]: http://phantomjs.org/
