---
title : "Extracting your Toggl data to Azure Blob Storage"
date: 2016-08-23 09:00
tags: [.net core, azure, .net, c#]
---

As a self-employed developer, I use multiple tools to enable me to easily do the boring job. One of those boring job is invoicing.

I normally invoice monthly and I use 2 tools for this. [Toggl](https://toggl.com) for time tracking and Excel to generate my invoice.

Yeah. I use Excel. It's easy and it works. The downside is that I have to do a lot of copy/paste. Extract data from Toggl, import it into Excel, replace the invoice numbers, generate the PDF, export to Dropbox for safekeeping, etc.

So, I've decided to try to automate the most of those task for the lowest possible cost. My first step will be to extract the raw data for which I bill my clients.

If you are interested, you can find the solution [on GitHub](https://github.com/NovawebSolutions/Invoicing/). The project name is `TogglImporter`.

Here's the `Program.cs`

```csharp
using System;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.PlatformAbstractions;

namespace TogglImporter
{
    public class Program
    {
        public static void Main(string[] args)
        {
            Task.WaitAll(Run(args));
        }

        public static async Task Run(string [] args)
        {
            Console.WriteLine("Starting Toggl Import...");
            var configuration = new ConfigurationBuilder()
                .SetBasePath(PlatformServices.Default.Application.ApplicationBasePath)
                .AddCommandLine(args)
                .AddJsonFile("appsettings.json")
                .AddJsonFile("appsettings.production.json", true)
                .Build();


            var queries = new Queries(configuration["togglApiKey"]);
            var storage = new CloudStorage(configuration["storageAccount"], "toggl-rawdata");
            Console.WriteLine("Initializing storage...");
            await storage.InitializeAsync();

            Console.WriteLine("Saving workspaces to storage...");
            var workspaces = await queries.GetWorkspacesAsync();
            var novawebWorkspace = workspaces.First(x => x.Name == "Novaweb");
            await storage.SaveWorkspace(novawebWorkspace);
            await Task.Delay(250);

            Console.WriteLine("Saving clients to storage...");
            var clients = await queries.GetWorkspaceClientsAsync(novawebWorkspace.Id);
            await storage.SaveClients(clients);
            await Task.Delay(250);

            Console.WriteLine("Saving projects to storage...");
            var projects = await queries.GetWorkspaceProjectsAsync(novawebWorkspace.Id);
            await storage.SaveProjects(projects);
            await Task.Delay(250);

            Console.WriteLine("Saving time entries to storage...");
            const int monthsToRetrieve = 2;
            for (int monthsAgo = 0; monthsAgo > -monthsToRetrieve; monthsAgo--)
            {
                var timeEntries = await queries.GetAllTimeEntriesForXMonthsAgoAsync(monthsAgo);
                await storage.SaveTimeEntriesAsync(DateTime.UtcNow.AddMonths(monthsAgo), timeEntries);
                await Task.Delay(500);
            }

            Console.WriteLine("Toggl import completed.");
        }
    }
}
```

### What's happening?
First, I have an object called `Queries` that will take a Toggl API Key and will query their API. Simple HTTP Client requests that returns objects that are already deserialized.

Then I have an object called `CloudStorage` that will store those objects to specific area in the cloud. Those will enforce folder hierarchies and naming.

Finally, I delay after each request to ensure I'm not overloading their API. The last thing you want to do, is having them shut it all down.

### What's being exported?

I export my Workspace, Clients, Projects as well as every time entries available for the last 2 months. I do those in multiple requests because this specific API will limit the amount of entries to 1000 and does not support paging.

If any of this can be helpful, let me know in the comments.
