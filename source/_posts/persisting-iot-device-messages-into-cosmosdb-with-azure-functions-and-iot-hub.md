---
title : "Persisting IoT Device Messages into CosmosDB with Azure Functions and IoT Hub"
date: 2018-02-27 16:30:00
tags: [iot, serverless, azure, cosmosdb]
---

If you are doing IoT, you are generating data. Maybe even lots of data. If you are doing API calls on each device to store them directly, you are doing yourself a disservice. If you are using something different as an event handler, things are better. If you are like me, you're using Azure IoT Hub to ingest the events. 

## IoT Hub

IoT Hub is a great way to ingress data from thousands of devices without having to create a scalable API to handle all of them. Since you don't know if you will be receiving one event per hour or 1000 events per seconds, you need a way to gather all this. However, those are just messages. 

You want to be able to store all your events efficiently whether it's 100 events or a billion.

<!-- 
 TODO : Example on how to create an IoT Hub?
-->

## Azure Functions ⚡

You could always spawn a VM or even create an App Service application and have jobs dequeue all those messages. There's only one issue. What happens when your devices stop sending events? Maybe you're running a manufacturing company that only operates 12 hours a day. What happens during those other 12 hours? You are paying for unused compute. What happens to the week where things need to run 15 hours instead of 12? More manual operations. 

That's where serverless becomes a godsend. What if I tell you that you'd only pay for what you use? No usage, no charge. In fact, Azure Functions comes in with 1 million execution for free. Yes, single function execution. You pay pennies per million executions. 

Azure Functions is the perfect compute construct for use in IoT development. It allows you to bring in massive compute power only when you need it. 

# Storing the events

We have our two building blocks in place. IoT Hub to ingest event, Azure Functions to process them. Now the question remains where do I store them?

I have two choices that I prefer. 

* Azure Table Storage ([can be done natively through endpoints](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage?WT_mc.id=blog-marouill&utm_source=marouill&utm_medium=blog))
* Azure Cosmos DB

Now let's assume a format of messages that are sent to our IoT Hub. That will serve as a basis for storing our events.

```json
{
    "machine": {
        "temperature": 22.742372309203436,
        "pressure": 1.198498111175075
    },
    "ambient": {
        "temperature": 20.854139449705436,
        "humidity": 25
    },
    "timeCreated": "2022-02-15T16:27:05.7259272Z"
}
```

## CosmosDB

CosmosDB allows you to store a massive amount of data in a geo-distributed way without flinching under load. Besides its different [consistency model](https://docs.microsoft.com/en-us/azure/cosmos-db/consistency-levels?WT_mc.id=blog-marouill&utm_source=marouill&utm_medium=blog) and multiple [APIs](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-introduction?WT_mc.id=blog-marouill&utm_source=marouill&utm_medium=blog), it is a fantastic way to store IoT events and still be able to query them easily.

So let's assume we receive the previously defined message through an Azure Function.

Let's create our Function. We'll be using the CSX model that doesn't require Visual Studio to deploy. We can copy/paste directly this code in the portal.

```csharp
#r "Newtonsoft.Json"

using System;
using Newtonsoft.Json.Linq;

public static void Run(string myIoTHubMessage, out object outDocument, TraceWriter log)
{
    dynamic msg = JObject.Parse(myIoTHubMessage);
    outDocument = new  {timeCreated = msg.timeCreated, temperature = msg.machine.temperature};
}
```

### Inputs

Then, we need to define our inputs. This is done with the Integrate option below our function.

![Azure Functions IoT Hub Inputs](/posts/files/iot-save-events/iothub-function-input.PNG)

In this section, we define our function parameter that matches with our written function. I also create a new event hub connection.

### Output

Now we need to define where things are going to go. In our case, I'm setting a Cosmos DB Output. 

![Azure Functions CosmosDB Output](/posts/files/iot-save-events/iothub-function-output.PNG)

In this section, I created a new connection to my Cosmos DB account where save our messages. As you can see, if you check the right checkbox, you don't need to create any collection or databases manually.

### On Automating 

As you can see, I'm being all fancy and creating everything through a Portal UI. Everything I've done can be replicated with an [ARM Template]() that will allow you to provision your different resources and bind your connection strings together. 

If you are interested in seeing a way to deploy this through the command line, please let me know in the comments.

### Results

After everything has been hooked up together, I sent a few manual event on my IoT Hub and looked into my Cosmos DB account.

![Azure Functions CosmosDB Result](/posts/files/iot-save-events/iothub-function-saved.PNG)

Amazing!

## Want more?

So what we just saw was a very cheap and scalable way to receive a ton of events from thousands of devices and store them in Cosmos DB. This will allow us to either create reports in Power BI, consume them in Machine Learning algorithms or stream them through SignalR to your administrative dashboard.

What would you be interested next? Let me know in the comment section.