---
title : "Decoupling with Azure Functions bindings and Storage Queues triggers"
date: 2017-08-30 13:15:00
tags: [azure, serverless]
---

If you want to decouple your application from your logic, it's always useful to separate events happening and actual actions.

This technique is also known as Command Query Responsibility Segregation or CQRS for short. The main principle is to allow events to be queued in a messaging service and have a separate mechanism to unload them.

Traditionally, we would do something like this with Rabbit MQ and maybe a command line application that would run on your machine. With modern cloud based applications, we would create an App Service under Azure and have a WebJob handle all those messages.

The main problem with this approach is that your WebJobs scales with your application. If you want to process messages faster, you have to scale up. How unwieldy!

## Here comes Serverless with Azure Functions

The thing with Azure Functions is that you can [define a `QueueTrigger`][QueueTrigger] and [Table Output Binding][TableOutputBinding], implement your business logic and be on your way.

That's it! Here's an example.


```csharp
public static class MoveQueueMessagesToTableStorage
{
    [FunctionName("MoveQueueMessagesToTableStorage")]        
    public static void Run([QueueTrigger("incoming-animals")]string myQueueItem,
        [Table("Animals")]ICollector<Animal> tableBindings, TraceWriter log)
    {           
        log.Info($"C# Queue trigger function processed: {myQueueItem}");

        // todo: implement my business logic to add or not the defined rows to my Table Storage

        tableBindings.Add(JsonConvert.DeserializeObject<Animal>(myQueueItem));            
    }
}

public class Animal : TableEntity
{
    public Animal()
    {
        PartitionKey = "Default";
    }
    public string Race { get; set; }
    public string Name { get; set; }
}
```

## What about poison messages?

First, what's a poison message? A poison message is a message that fails to be processed correctly by your logic. Either the message was malformed, or your code threw an exception. Whatever the reason, we need to take care of that!

Let's add another function to handle those *extraordinary* cases.

```csharp
[FunctionName("MoveQueueMessagesToTableStorage-Poison")]
public static void HandlePoison([QueueTrigger("incoming-animals-poison")]string myQueueItem, TraceWriter log)
{
    log.Error($"C# Poison Queue trigger function on item: {myQueueItem}");

    // handle cases where the message just can't make it.
}
```

Waaaaaait a minute... what's this `incoming-animals-poison` you tell me? That, my friend, [is automatically created][PoisonMessage] when queue trigger fails five times in a row.

Did we just handle poison messages by just creating a single function? Yes.

## Cost

So by now, you're probably asking me:

> Well, how much is this serverless goodness going to cost me?

That's the beauty of it. On a consumption plan? Zero. You only pay for what you use and if you use nothing or below the [free quota][FreeQuota]? 0$

## Takeaways

Creating a decoupled architecture is easier with a serverless architecture. Cost is also way easier to manage when you don't need to pre-provision resources and are only paying for what you are executing.

[QueueTrigger]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-storage-queue?WT.mc_id=maximerouiller-blog-marouill
[TableOutputBinding]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-storage-table?WT.mc_id=maximerouiller-blog-marouill
[PoisonMessage]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-storage-queue#handling-poison-queue-messages?WT.mc_id=maximerouiller-blog-marouill
[FreeQuota]: https://azure.microsoft.com/en-ca/pricing/details/functions/?WT.mc_id=maximerouiller-blog-marouill
