---
title : "Uploadings files to Storage in batches, two ways"
date: 2018-12-14 13:45:00
tags: [azure, static content, storage, tool]
---

I love static sites. They are cheap, easy to maintain, and are total non-issue in terms of security.

What? You hacked my site? Let me delete everything, reupload my files, and... we're done. Okay... not totally true but you get the point.

Having the source of truth away from your deployment is a big relief. Your deployment having no possible actions on your source of truth is even better.

Okay, where am I going with this? Maybe you saw the [announcement](https://azure.microsoft.com/blog/static-websites-on-azure-storage-now-generally-available/?WT.mc_id=personal-blog-marouill) a few days ago that Static Sites on Azure Storage went General Availability (internally, we call this **GA** because we love acronyms).

Now let me tell you something else that you may not have picked up that I consider GA (Greatly Amazing).

## Uploading files to Storage

Uploading files to storage is an operation that be done in many ways. Maybe you go through the portal and upload your files one by one. I don't know about you, but, I got things to do. I can't spend the day uploading my site like this. So we need alternatives.

### Command line Interface (CLI, another acronym)

So how do I get you, dear reader, to be able to upload it from anywhere in the world including your phone. Why? No time to explain. We're more interested in the HOW?

```bash
az storage blob upload-batch -d MyContainer --account-name MyStorageAccount -s ./generated/ --pattern *.html --if-unmodified-since 2018-08-27T20:51Z
```

Want to customize it a little? [Check out the docs](https://docs.microsoft.com/cli/azure/storage/blob?view=azure-cli-latest&WT.mc_id=personal-blog-marouill#az-storage-blob-upload-batch). It's easy. It's amazing. It works in [Azure Cloud Shell](https://shell.azure.com?WT.mc_id=personal-blog-marouill).

### Graphical User Interface (GUI, see? we love them)

You know what I think is even better in demos? A nice graphical interface. No, I'm not about to recommend you install [Visual Studio 2019](https://visualstudio.microsoft.com/vs/preview/?WT.mc_id=personal-blog-marouill) to edit some markdown files and publish to Azure Storage. May look fun but... it's still Windows only. I want you to be able to do that operation everywhere.

Let's take something cross platform. Let's take something that isn't as polarizing. Let's take an IDE that's built on Electron.

Let's use Azure Storage Explorer. *record scratch*

#### Azure Storage Explorer

[Download it](https://azure.microsoft.com/features/storage-explorer/?WT.mc_id=personal-blog-marouill). Install it. Open it. Login with your account.

Bam. We're here.

![Image of Azure Storage Explorer initial state](/posts/files/staticstorageexplorer/StorageExplorer.png)

So how do I upload my blog to my storage account now? Well, someone told me once that an image is worth a thousand words. 

An animated GIF is therefor priceless.

![priceless animated gif](/posts/files/staticstorageexplorer/priceless.gif)

Did you see that? Drag and drop.

## Getting started

So if you got yourself a static site using Azure Storage, I want you to know that whatever tools you're using, this scenario works out of the box.

It is supported. You are in good hands. Do you have feedback on the Azure Static Site? Let me know [on Twitter](https://twitter.com/MaximRouiller) and we'll talk.

Here's a summary of all the links in this post.

- [Azure Storage Static Site General Availability Announcement](https://azure.microsoft.com/blog/static-websites-on-azure-storage-now-generally-available/?WT.mc_id=personal-blog-marouill)
- [Downloading Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/?WT.mc_id=personal-blog-marouill)
- [Azure CLI Storage Batch Upload reference](https://docs.microsoft.com/cli/azure/storage/blob?view=azure-cli-latest&WT.mc_id=personal-blog-marouill#az-storage-blob-upload-batch))
- [Azure Cloud Shell](https://shell.azure.com?WT.mc_id=personal-blog-marouill)