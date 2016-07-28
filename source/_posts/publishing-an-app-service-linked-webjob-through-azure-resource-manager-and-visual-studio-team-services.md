---
title : "Publishing an App Service linked WebJob through Azure Resource Manager and Visual Studio Team Services"
date: 2016-07-27 12:00
tags: [azure, webjobs]
---

### Assumptions

I assume that you already have a workflow with Visual Studio Team Services that deploy your Web Application as an App Service.

I assume that you are also deploying with an Azure Resource Group project that will automatically provision your application.

### Creating the WebJobs

Well... that part is easy. You right click on the application that will be hosting your WebJob and you select `Add > New Azure WebJob Project`.

![New Azure WebJobProject](/posts/files/publishing-webjobs/menu.png)

Once you click on it, you should see the following Wizard. At that point, you have a wide variety of choices on how and when to run your WebJob. Personally, I was looking for a way to run tasks on a schedule every hours forever.

You could have a Continous job that will respond to trigger, one time jobs, recurring jobs with and end date. It's totally up to you at that point.

![New Azure WebJobProject](/posts/files/publishing-webjobs/add-webjob.png)

Once you press `OK`, a new projet will be created and your Web App will have a new file named `webjobs-list.json` added under `Properties`. Let's look at it.

```json
{
  "$schema": "http://schemastore.org/schemas/json/webjobs-list.json",
  "WebJobs": [
    {
      "filePath": "../MyWebJob/MyWebJob.csproj"
    }
  ]
}
```

That is the link between your WebApp and your WebJobs. This is what will tell the build process on VSTS that it also needs to package the WebJob with this website so that Azure can recognize that it has WebJobs to execute.

### Configuring the WebJob

#### Scheduling

By default, you will have a `webjob-publish-settings.json` created for you. It will contain everything you need to run the task at the set interval.

However, the `Program.cs` should look like this for task that should be run on a schedule.

```csharp
public class Program
{
    static void Main()
    {
        var host = new JobHost();
        host.Call(typeof(Functions).GetMethod("MyTaskToRun"));
        //host.RunAndBlock();
    }
}
```

If the `RunAndBlock` command is present, the process will be kept alive and the task won't be able to start at the next scheduled run. So remove it.

As for the code itself, flag it with `NoAutomaticTrigger`.
```csharp
public class Functions
{
    [NoAutomaticTrigger]
    public static void MyTaskToRun()
    {
        Console.WriteLine("Test test test");
    }
}
```

#### Continuous

This is the perfect mode when you want to respond to events like new message being sent from the Queue.

In this scenario, ensure that you have `RunAndBlock`. This is a polling mechanism and if your WebJob isn't running, it's not running events.

```csharp
public class Program
{
    static void Main()
    {
        var host = new JobHost();
        host.RunAndBlock();
    }
}
```

```csharp
public class Functions
{
    public static void ProcessMessage([QueueTrigger("MyQueue")] string message, TextWriter log)
    {
        //TODO: process message
    }
}
```

### Enabling the Logs with your Storage Account through ARM templates

That's the last part that needs to make sure your WebJob is running properly.

By default, they are running part in the same instance as your website. You need a way to configure your storage account within your ARM template. You really don't want to hardcode your Storage account within your WebJob anyway.

So you just need to add this section under your WebApp definition to configure your connection strings properly.

```json
{
    "resources": [
        {
          "apiVersion": "2015-08-01",
          "type": "config",
          "name": "connectionstrings",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', variables('myWebApp'))]"
          ],
          "properties": {
            "AzureWebJobsDashboard": {
              "value": "[Concat('DefaultEndpointsProtocol=https;AccountName=',variables('MyStorageAccount'),';AccountKey=',listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('MyStorageAccount')), providers('Microsoft.Storage', 'storageAccounts').apiVersions[0]).keys[0].value)]",
              "type": "Custom"
            },
            "AzureWebJobsStorage": {
              "value": "[Concat('DefaultEndpointsProtocol=https;AccountName=',variables('MyStorageAccount'),';AccountKey=',listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('MyStorageAccount')), providers('Microsoft.Storage', 'storageAccounts').apiVersions[0]).keys[0].value)]",
              "type": "Custom"
            }
          }
        }]
    }
```
