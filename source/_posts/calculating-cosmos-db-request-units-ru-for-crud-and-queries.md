---
title : "Calculating Cosmos DB Request Units (RU) for CRUD and Queries"
date: 2018-03-19 11:30:00
tags: [azure, cosmosdb, performance]
---

Video version also available

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/4lYx-aG3sFM?rel=0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

Cosmos DB is a globally distributed database that offers single-digit-millisecond latencies on multiple models. That's a lot of power under the hood. As you may be tempted to use as much of it as possible, you have to remember that you are billed for what you use.

Cosmos DB they measure your actual usage of the service on [Request Units (RU)](https://docs.microsoft.com/en-us/azure/cosmos-db/request-units?WT.mc_id=personal-blog-marouill#introduction). 

## What are Cosmos DB Request Units (RU)?

Request units are a normalized number that represents the amount of computing power (read: CPU) required to serve the request. Inserting new documents? Inexpensive. Making a query that sums up a field based on an unindexed field? Ccostly.

By going to the [Cosmos DB Capacity Planner tool](https://www.documentdb.com/capacityplanner?WT.mc_id=personal-blog-marouill), we can test from a JSON sample document how many RUs are required based on your estimated usage. By uploading a simple document and setting all input values to 1 (create, read, update, delete) we can see which operations are relatively more expensive than others.  

```none
Create RUs:  5.71
  Read RUs:  1.00
Update RUs: 10.67
Delete RUs:  5.71
```

Those are the number at the time of writing this blog post and may change in the future. Read is for a single document. Queries work differently.

## Tracking Request Unit (RU) usage

Most operation with the `DocumentClient` (SQL API) will return you a model that will allow you to see how much RU we use. Here are the four basic operations and how easy it is to retrieve their respective Request Unit.

### Create

To retrieve the amount of Request Unit used for creating a document, we can retrieve it like this.

```csharp
var collectionUri = UriFactory.CreateDocumentCollectionUri(database.Id, documentCollection.Id);
var result = await client.CreateDocumentAsync(collectionUri, new { id = "1", name = "John"});
Console.WriteLine($"RU used: {result.RequestCharge}");
```
### Update

We can also retrieve it while updating a document.

```csharp
var document = new { id = "1", name = "Paul"};
var result = await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(database.Id, documentCollection.Id, document.id), document);
Console.WriteLine($"RU used: {result.RequestCharge}");
```

### Delete

Finally, figuring out the amount of RU used for deleting a document can be done like so.

```csharp
var result = await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(database.Id, documentCollection.Id, "1"));
Console.WriteLine($"RU used: {result.RequestCharge}");
```

This is quite easy. Right? Let's go onto Queries.


## Calculating the Request Unit (RU) of Cosmos DB Queries

That's where things get a little more complicated. Let's build a query that returns the top 5 documents and retrieve the results. The default API usage makes it very easy for us to retrieve a list of elements but not the Request Units. 

```csharp
var documentQuery = client.CreateDocumentQuery(collectionUri).Take(5);

// materialize the list.. but we lose the RU
var documents = documentQuery.ToList();
```

Here's why it's difficult to retrieve the RU in this scenario. If I do a `ToList`, it will return a generic list (`List<T>`) on which I can't append more properties. So, we lose the Request Units while retrieving the documents. 

Let's fix this by rewriting this query. 

```csharp
var documentQuery = client.CreateDocumentQuery(collectionUri).Take(5).AsDocumentQuery();

double totalRU = 0;
List<dynamic> allDocuments = new List<dynamic>();
while (documentQuery.HasMoreResults)
{
    var queryResult = await documentQuery.ExecuteNextAsync();
    totalRU += queryResult.RequestCharge;
    allDocuments.AddRange(queryResult.ToList());
}
```

If all you wanted was the code, what's above will do the trick for you. If you want to understand what happens, stick around.

### The explanation

Cosmos DB will never return 1 million rows to you in one response. It will page it. It's why we see a pattern similar to an `Enumerator`.

The first thing we do is move the query from an IQueryable to an [`IDocumentQuery`](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.documents.linq.idocumentquery-1?view=azure-dotnet&WT.mc_id=personal-blog-marouill). Using this method enables us to access the `ExecuteNextAsync` method and the `HasMoreResults` property. With just those two, we can now get a separate `FeedResponse<T>` for each *page* of our query. It's now obvious that if you try to extract all the data from a collection, you are using RUs for each page of result. 

## Next Steps

Want to give it a try? Never tried Cosmos DB before? 

You can [get 7 day of free, no credit card, no subscription access](https://azure.microsoft.com/en-us/try/cosmosdb/?WT.mc_id=personal-blog-marouill), and no questions asked.

Then, once you have a free database, try one of the [5 minutes quickstart](https://docs.microsoft.com/en-us/azure/cosmos-db/?WT.mc_id=personal-blog-marouill) in the language that you want.

Need more help? Ask me [on Twitter](https://twitter.com/MaximRouiller). I'll be happy to help!