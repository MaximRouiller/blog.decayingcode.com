---
title : ""
date: 
tags: []
---

So, [yesterday I talked about going Static](/posts/breaking-the-shackles-of-server-frameworks-with-static-content) but I didn't talk about how to deploy it.

My main hoster is Azure. 

## Needed feature to host a blog

* File hosting
* Custom domain
* Support for default file resolution (aka: `default.html`, `index.html`, etc.)

## App Services

[Complete pricing detail here](https://azure.microsoft.com/en-us/pricing/details/app-service/)
Cost: $10/months for Shared

You want to go with at least Shared. The main reason is that you're going to need that domain and the free tier doesn't allow you to set your domain.

This allows you to reserve compute but won't guarantee that your website will always be "warm". This would matter if your application was .NET and needed to be JIT. But I have a good news for you, we're going static. We don't even need the .NET pipeline.

### How to deploy 

```none
????
```

## Azure Functions

[Complete pricing detail here](https://azure.microsoft.com/en-us/pricing/details/functions/)
Cost: Storage

### How to deploy

This allow us to create a storage account and upload our blob content to it. This is going to be reused later.

```powershell
az group create --name staticblog-test -l eastus
az storage account create --name hugoblog2 -g staticblog-test --sku Standard_LRS

$key = az storage account keys list -n hugoblog2 -g staticblog-test --query [0].value -o tsv
az storage container create --name content --public-access container --account-name hugoblog2 --account-key $key

cd <output directory of your static site>
az storage blob upload-batch -s . -d content --account-name hugoblog2 --account-key $key --max-connections 20
```

#### Provision the function

```powershell 
az functionapp create -n hugoblogapp -g staticblog-test -s hugoblog2 -c eastus2
```

Then, we create a `proxies.json` file to configure URL routing to our blob storage.

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

Note: You will need to set a [deployment user]() before pushing to source control.

```powershell
az functionapp deployment source config-local-git --name hugoblogapp -g staticblog-test
$git = az functionapp deployment source config-local-git --name hugoblogapp -g staticblog-test -o tsv
git remote add azure $git
git push azure master
```

After, we need to add that file to this repository.

```powershell
code proxies.json # copy/paste the content above and change your blob url
git add .
git commit -m "adding proxies"
git push azure master
```