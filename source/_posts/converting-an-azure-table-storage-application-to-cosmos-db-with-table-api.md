---
title : "Converting an Azure Table Storage application to Cosmos DB with Table API"
date: 2018-03-13 13:30:00
tags: [azure, storage, cosmosdb]
---

Converting an application that is using Azure Table Storage to Cosmos DB is actually pretty easy to do.

Azure Table Storage is one of the oldest Microsoft Azure storage technology7 out there and lots of applications still uses it. But, what if you need to go global and have your data accessed in a performant way with better SLAs that are guaranteed by the standard Storage Account SLAs? 

Cosmos DB allows you to effortlessly transition from one to the other by one single change in your code.

## Previous Code

Here's how we would normally build a Azure Storage Client.

```csharp
private async Task<CloudTable> GetCloudTableAsync()
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(configuration.GetConnectionString("initial"));
    var tableClient = storageAccount.CreateCloudTableClient();
    CloudTable table = tableClient.GetTableReference("mytable");
    await table.CreateIfNotExistsAsync();
    return table;
}
```

Whereas `initial` connection string is represented as this:

```none
DefaultEndpointsProtocol=https;AccountName=<ACCOUNT NAME>;AccountKey=<KEY>;EndpointSuffix=core.windows.net
```

## Cosmos DB Code

Here's how we would create the new `CloudTable` when using Cosmos DB.

```csharp
private async Task<CloudTable> GetCloudTableAsync()
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(configuration.GetConnectionString("destination"));
    var tableClient = storageAccount.CreateCloudTableClient();
    CloudTable table = tableClient.GetTableReference("mytable");
    await table.CreateIfNotExistsAsync();
    return table;
}
```

Whereas `destination` connection is represented as this:
```none
DefaultEndpointsProtocol=https;AccountName=<ACCOUNT NAME>;AccountKey=<KEY>;TableEndpoint=https://MYCOSMOS.table.cosmosdb.azure.com:443/;
```

## Difference in your code

And that's it. A single connection string change and you've gone from the good ol' Table Storage to [multiple consitency levels](https://docs.microsoft.com/en-us/azure/cosmos-db/consistency-levels?WT.mc_id=personal-blog-marouill), to [globally replicated](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-introduction?WT.mc_id=personal-blog-marouill) data in multiple regions.

## Difference in implementation

Of course, we went from 2 different implementations from 1 single API. There's bound to be differences. The [complete list](https://docs.microsoft.com/en-us/azure/cosmos-db/faq?WT.mc_id=personal-blog-marouill#develop-with-the-table-api) goes in details but it will end up being more expensive as Cosmos DB will preallocate your storage while your Storage Account will only allocate what you use. As much as it will end up being more expensive on Cosmos DB, you will also end up with better performance. 

## Try it now

If you want to try Cosmos DB, there's multiple ways. 

If you don't have an account or a credit-card, you can try it for free [right here](https://azure.microsoft.com/en-us/try/cosmosdb/?WT.mc_id=personal-blog-marouill).

If you want to not be limited by the subscribtion-less option, you can always get an [Azure Free Trial](https://azure.microsoft.com/en-us/free/?WT.mc_id=personal-blog-marouill) which includes free credits for Cosmos DB.