---
title : "Fixing Azure Functions and Azure Storage Emulator 5.8 issue"
date: 2019-03-19 09:05:00
tags: [serverless, azure, storage]
---

So you've updated Azure Functions to the latest version (2.X at the time of writing this), and nothing starts anymore.

Now, when you boot the Functions host, you get this weird error.

```none
[TIMESTAMP] A host error has occurred
[TIMESTAMP] Microsoft.WindowsAzure.Storage: Server encountered an internal error. Please try again after some time.
```

There happen to be [an issue opened on GitHub](https://github.com/Azure/azure-functions-host/issues/3795) that relates to Durable Functions and Azure Storage Emulator.

The thing is, it's not directly related to Azure Durable Functions. It's related, in my opinion, in a breaking change that happened in Azure Storage Emulator 5.8 ways of responding from its API.

If you want to fix that issue, merge the following setting in your `local.settings.json` file.

```json
{
  "Values": {
    "AzureWebJobsSecretStorageType": "files"
  }
}
```

This only applies when `"AzureWebJobsStorage": "UseDevelopmentStorage=true"`.

So why should we set that? There was a change that was introduced back in September 2018 when Azure Functions V2 was released. Azure Functions store your secret on disk before 2.0. Azure Functions, when [slot swapping environments](https://docs.microsoft.com/azure/app-service/deploy-staging-slots?WT.mc_id=personal-blog-marouill#swap-two-slots), swap the content of the disk including the secrets.

What this setting does is ensure that Functions stores secrets on your file system by default. It's expected behavior when using a local development environment.

If you want to read more, there is [an entire article on that behavior](https://github.com/Azure/azure-functions-host/wiki/Changes-to-Key-Management-in-Functions-V2).

## Conclusion

To fix the issue, you can either use the workaround or [update the Azure Storage Emulator to 5.9](https://docs.microsoft.com/azure/storage/common/storage-use-emulator?WT.mc_id=personal-blog-marouill).