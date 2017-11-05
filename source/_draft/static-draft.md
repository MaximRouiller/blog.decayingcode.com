---
title : ""
date: 
tags: []
---

So, [I recently talked about going Static](/posts/breaking-the-shackles-of-server-frameworks-with-static-content), but I didn't talk about how to deploy it.

My favorite host is Azure. Yes, I could probably go with different hosts, but Azure is just so easy to use that I don't see myself changing anytime soon.

What about I show you how to deploy a static site to Azure while keeping the cost at less than 1$ per months*?

**it's less than $1 if you have a low traffic site. Something like Scott Hanselman's blog would probably cost more to run.*

## Needed feature to host a blog

So to host a static blog, we need to have a few fundamental features

* File hosting
* Custom domain 
* Support for default file resolution (e.g., `default.html` or `index.html`)

With this shopping list in mind, let me show you how Static and Serverless can help you achieve crazy low cost. 

## Azure Functions (or rather, proxies)

### Cost (price in USD)

> Prices change. Pricing model too. Those are the numbers in USD at the time of writing.

Azure Storage is $0.03 per stored GB, $0.061 per 10,000 write operations and $0.005 per read operation. So if your blog stays under 1Gb and you get 10,000 views per months, we can assume between $0.05 and $0.10 per months in cost (CSS/JS/images).

Azure Functions provides the first 1 million execution for free. So let's assume 3 million requests, and it would bring us to $0.49.

Big total of $0.69 for a blog or static site hosted on a custom domain. 

### How to deploy

Now that we have our `/public` content for our Hugo blog from the last post, we need to push this into Azure Storage. One of the concepts is called Blobs (Binary Large Object). Those expose content on a separate URL that looks like this: `https://<StorageAccount>.blob.core.windows.net/<Container>/`

So uploading our root `index.html` will be accessible on `https://<StorageAccount>.blob.core.windows.net/<Container>/index.html`. We will need to do this for ALL files in our current `/public` directory

So as the URL shows, we'll need a storage account as well as the base container. The following script will create the required artifacts and upload the current folder to that URL.

```powershell
az group create --name staticblog-test -l eastus
az storage account create --name hugoblog2 -g staticblog-test --sku Standard_LRS

$key = az storage account keys list -n hugoblog2 -g staticblog-test --query [0].value -o tsv
az storage container create --name content --public-access container --account-name hugoblog2 --account-key $key

cd <output directory of your static site>
az storage blob upload-batch -s . -d content --account-name hugoblog2 --account-key $key --max-connections 20
```

So accessing the same file on that URL will now be accessible on `https://hugoblog2.blob.core.windows.net/content/index.html`.

Now that we have the storage taken care of, it's important to remember that although we can associate a custom domain to a Storage Account, the Storage account does not support default files and uploading files to root. So even if we could map it to a custom domain, we would still have the `/content/index.html` instead of `/index.html`. 

So this brings us to Azure Functions.

#### Provision the function

So we'll need to create a Function using the following code. 

```powershell 
az functionapp create -n hugoblogapp -g staticblog-test -s hugoblog2 -c eastus2
```

Then, we create a `proxies.json` file to configure URL routing to our blob storage. **WARNING**: This is ugly. I mean it. Right now, it can only match by url segment (like ASP.NET MVC), and it's not pretty. If you want a feature request, check with the [Azure Functions team on Twitter](https://twitter.com/AzureFunctions).

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "root":{
            "matchCondition": {"route": "/"},
            "backendUri": "https://hugoblog2.blob.core.windows.net/content/index.html"
        },
        "firstlevel": {
            "matchCondition": {"route": "/{level1}/"},
            "backendUri": "https://hugoblog2.blob.core.windows.net/content/{level1}/index.html"
        },
        "secondlevel": {
            "matchCondition": {"route": "/{level1}/{level2}/"},
            "backendUri": "https://hugoblog2.blob.core.windows.net/content/{level1}/{level2}/index.html"
        },
        "thirdlevel": {
            "matchCondition": {"route": "/{level1}/{level2}/{level3}/"},
            "backendUri": "https://hugoblog2.blob.core.windows.net/content/{level1}/{level2}/{level3}/index.html"
        },
        "fourthlevel": {
            "matchCondition": {"route": "/{level1}/{level2}/{level3}/{level4}/"},
            "backendUri": "https://hugoblog2.blob.core.windows.net/content/{level1}/{level2}/{level3}/{level4}/index.html"
        },
        "css": {
            "matchCondition": {"route": "/css/main.css"},
            "backendUri": "https://hugoblog2.blob.core.windows.net/content/css/main.css"
        },
        "rest": {
            "matchCondition": {"route": "{*restOfPath}"},
            "backendUri": "https://hugoblog2.blob.core.windows.net/content/{restOfPath}"
        }
    }
}
```

#### Hooking git

Now, this `proxies.json` file is not configurable via the command line so we'll need to create a git repository, add the file to it and deploy it using Git.

> Note: You will need to set a [deployment user](https://docs.microsoft.com/en-us/cli/azure/webapp/deployment/user?view=azure-cli-latest&wt.mc_id=default-blog-marouill) before pushing to source control.

The script below will configure Git for our Functions app, add a remote upstream to the current git repository and push the whole repository. This will start a deployment. 


```powershell
az functionapp deployment source config-local-git --name hugoblogapp -g staticblog-test
$git = az functionapp deployment source config-local-git --name hugoblogapp -g staticblog-test -o tsv
git remote add azure $git
git push azure master
```

If your file wasn't in there before, I'd be using vscode to create the file, add it, commit it and push it.

```powershell
code proxies.json # copy/paste the content above and change your blob url
git add .
git commit -m "adding proxies"
git push azure master
```

Now your proxy will automatically be picked up, and your Hugo Blog now works.

The same thing can be done with some slight modification to the proxies.

## What do we have now? 

We have a system where we can forward any requests to the Storage URLs.

It's clunky for now, but it's one way to do it with serverless. Imagine now that you want to add an API, authentication, queuing messages. All this? It is all possible from that same function.

If you are building a SaaS application, it's an excellent option to have with the least amount of building blocks.

## Future changes

Even though it's the "minimum amount of building blocks", I still think that it's way too high.

The [second highest uservoice](https://feedback.azure.com/forums/217298-storage/suggestions/6417741-static-website-hosting-in-azure-blob-storage) that they have is about supporting complete static site without any workarounds.

I highly invite you to vote on it. It would add support for default files and other necessary features. Once implemented, this blog post will be unnecessary.

## What next?

Thinking creatively with Azure allows you to perform some great cost saving. If you keep this mindset, Azure will become a set of building blocks on which you will build awesomeness. 