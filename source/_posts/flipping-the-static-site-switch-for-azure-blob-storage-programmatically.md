---
title : "Flipping the static site switch for Azure Blob Storage programmatically"
date: 2018-12-10 14:10:00
tags: [azure, static content, serverless]
---

As a few of you knows already, I **REALLY** love static sites.

So when I read the news that [Azure Blob Storage enabled Static Site in Public Preview][StaticWebsite] back in June, how would you classify my reaction?

<iframe src="https://giphy.com/embed/5GoVLqeAOo6PK" width="480" height="375" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/excited-screaming-jonah-hill-5GoVLqeAOo6PK">via GIPHY</a></p>

Right away, I wanted to automate this deployment. Then began my search into Azure Resource Management (ARM) templates. I search left and right and could not find an answer.

Where was this mysterious setting hiding?

## Finding the switch

If the switch wasn't in the ARM template, it must be somewhere else.

Since I tend to rush myself into coding, it's the perfect moment to breathe and read. Let's read [the announcement][StaticWebsite] once again:

> This feature set is supported by the most recent releases of the Azure Portal, .Net Client Library (version 9.3.0), Java Client Library (version 8.0.0), Python Client Library (version 1.3.0), Node.js Client Library (version 2.10.0), Visual Studio Code Extension (version 0.4.0), and CLI 2.0 (extension version 0.1.3).

So, it's possible to do it in code. On top of that, there are at least 6 versions of this code in multiple languages that set that property. Wow. Way to rush too fast into code Maxime (imagine slow claps here).

By going into the .NET Client Library's GitHub, and searching for `static website` led me to [those results](https://github.com/Azure/azure-storage-net/search?q=static+website&unscoped_q=static+website).

The results may not look like much, but it mentions `Service Properties`. This is definitely not an ARM concept. Let's do a quick search for `azure storage service properties`.

Where does it lead us? Just like every good Google search should lead. [To the docs](https://docs.microsoft.com/rest/api/storageservices/set-blob-service-properties?WT.mc_id=maximerouiller-blog-marouill).

Wow. So I don't even need to do a REST API or sacrifice a few gummy bears to get it?

## Implementation

```csharp

CloudStorageAccount storageAccount = CloudStorageAccount.Parse("<connectionString here>");
var blobClient = storageAccount.CreateCloudBlobClient();
ServiceProperties blobServiceProperties = new ServiceProperties();
blobServiceProperties.StaticWebsite = new StaticWebsiteProperties
{
    Enabled = true,
    IndexDocument = "index.html",
    ErrorDocument404Path = "404.html"
};
await blobClient.SetServicePropertiesAsync(blobServiceProperties);
```

Just like that, the service is now enabled and able to serve HTTP request on a URL looking like this.

`https://mystorageaccount.z00.web.core.windows.net/`

Next post? Custom domain? Yay or nay?

## More Resources

If you want to read more about how it works, I would recommend the following resources:

* [Host a static website](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website?WT.mc_id=maximerouiller-blog-marouill)
* [Configure a custom domain name for your Azure Storage Account](https://docs.microsoft.com/azure/storage/blobs/storage-custom-domain-name?WT.mc_id=maximerouiller-blog-marouill)

[StaticWebsite]: https://azure.microsoft.com/blog/azure-storage-static-web-hosting-public-preview/?WT.mc_id=maximerouiller-blog-marouill