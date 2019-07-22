---
title : "GO GO Hugo Blog to Azure Storage"
date: 2017-11-09 11:11:11
tags: [azure, storage, hugo, static content]
---
[Previously](/post/hosting-static-site-for-cheap-on-azure-with-storage-and-serverless), we saw how to serve our static site out of blob storage.

The thing is, you'd still need to generate the actual HTML on a computer with all the tools installed. Well, that's no fun.

What if we could generate all of this dynamically?

Last time, we had a git repository with our proxies into it. Now's the time to add the whole root of our Hugo blog project. I would add `/public` to our ignore file as we'll be regenerating them anyway. 

>**Make sure that you do not include files with passwords, keys or other valuable data.**

I am using Hugo here, but any static site renderer that can run in a Windows Environment as a standalone executable or on a [list of supported language](https://docs.microsoft.com/en-us/azure/app-service/?WT.mc_id=personal-blog-marouill) will run fine.

## Minimum requirements before going further

### Hugo Executable
If you are going to follow this tutorial using Hugo, please make sure that you have the [stand-alone executable version](https://github.com/gohugoio/hugo/releases) for Windows downloaded. Also, **make sure to add it to our git repository in `/tools`**. We should now have `/tools/hugo.exe` present.

### AzCopy
Then, install the [latest version of AzCopy](http://aka.ms/downloadazcopy?WT.mc_id=personal-blog-marouill). I didn't find a way to get the newest version other than by the installer.

It installs by default under `C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy`. Copy all the DLLs and `AzCopy.exe` under our `/tools` folder. We'll need it very soon.

## Custom deployment in Azure

When we deploy as we did previously with an Azure hosted git repository, there are default behaviors applied to deployments. Mostly, it's copy/pasting the content and using it as our application.

But, we can do more. We can customize it.

The first step is installing [`kuduscript`](https://github.com/projectkudu/KuduScript) and generate a basic deployment script.

```bash
npm install -g kuduscript
# generates a powershell script for custom deployment
kuduscript --basic -t posh -y
```

The generated deployment script is useless to us. We'll empty it. However, I wanted you to see its content first. We could forgo `kuduscript` altogether because we're just going to write our script but, it's important to notice what this script is doing and how to generate it. It allows you to customize your whole deployment process if you ever need to do that kind of thing without a specialized tool like Visual Studio Team Services.

So, the lesson's over. Let's empty out that file and paste the following inside.

```powershell
# Generates our blog to /public
.\tools\hugo.exe -t minimal

# Connection string associated with the blob storage. Can be input manually too.
$blobStorage = $env:AzureWebJobsStorage 

# We extract the key below
$accountKey = ""
$array = $blobStorage.Split(';')
foreach($element in $array)
{
    if($element.Contains('AccountKey'))
    {
        $accountKey = $element.Replace("AccountKey=", "")
    }
}

if($accountKey -ne "")
{
    # Deploy to blob storage
    .\tools\AzCopy.exe /Source:.\public /Dest:https://hugoblog2.blob.core.windows.net/content /DestKey:$accountKey /SetContentType /S /Y
}
else 
{
    Write-Host "Unable to find Storage Account Key"
}
```

Let's send this to our Azure git repository that we set earlier.

```powershell
git add .
git commit -m "deploying awesomeness by the bucket"
git push azure master
```

### Resulting output

As soon as you hit `Enter` on this last command, you should be receiving these answers from the remote:

```none
remote: Updating branch 'master'.
remote: ....
remote: Updating submodules.
remote: Preparing deployment for commit id 'f3c9edc30c'.
remote: Running custom deployment command...
remote: Running deployment command...
remote: .............
remote: Started building sites ...
remote: ...................................
remote:
remote: Built site for language en:
remote: 0 draft content
remote: 0 future content
remote: 0 expired content
remote: 305 regular pages created
remote: 150 other pages created
remote: 0 non-page files copied
remote: 193 paginator pages created
remote: 0 categories created
remote: 71 tags created
remote: total in 39845 ms
remote: .......................
remote: [2017/11/09 15:16:21] Transfer summary:
remote: -----------------
remote: Total files transferred: 652
remote: Transfer successfully:   652
remote: Transfer skipped:        0
remote: Transfer failed:         0
remote: Elapsed time:            00.00:00:26
remote: Running post deployment command(s)...
remote: Syncing 0 function triggers with payload size 2 bytes successful.
remote: Deployment successful.
```


From that little script, we managed to move our static site content generation from your local machine to the cloud. 

And that's it. Every time you will `git push azure master` to this repository, the static site will be automatically generated and reuploaded to Azure Blob Storage.

Is there more we can do? Anything else you would like to see?

Let me know in the comments below!