---
title: "How NOT to copy one BlockBlob to another in Azure"
date: 2016-06-22 12:00:00
tags: [azure,storage]
---

So I came onto this piece of code that was trying to replicate a BlockBlob from one place to another.

```csharp
destinationBlob.Properties.ContentType = sourceBlob.Properties.ContentType;

using (var stream = new MemoryStream())
{
    sourceBlob.DownloadToStream(stream);
    stream.Seek(0, SeekOrigin.Begin);
    destinationBlob.UploadFromStream(stream);
}
```

### The problem

First, it will download the whole block locally to re-upload it.

That is pretty awesome but what if your file is 500Mb? Yeah. It's completely loaded into memory and your process memory usage will explode. Worse, if your process is 32bits, you might actually run into `OutOfMemoryException` sooner rather than later.

### The solution

Azure already provides a way to copy data from one blob to another. All this code was replaced by this single line of code.

```csharp
newBlob.StartCopy(oldBlob);
```

If you don't know how to do something in Azure, looking at the API or asking around might just save you a few hours of troubleshooting.
