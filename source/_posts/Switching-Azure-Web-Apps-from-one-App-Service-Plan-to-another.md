---
title: "Switching Azure Web Apps from one App Service Plan to another"
date: 2015-03-30 17:00:05
tags: [azure,powershell]
---

So I had to do some change to App Service Plan for one of my client. The first thing I was looking for was to do it under the [portal](http://portal.azure.com). A few clicks and I'm done!

But before I get into why I need to move one of them, I'll need to tell you about why I needed to move 20 of them.

### Consolidating the farm

First, my client had a lot of WebApps deployed left and right in different &quot;Default&quot; ServicePlan. Most were created automatically by scripts or even Visual Studio. Each had different instance size and difference scaling capabilities.

We needed a way to standardize how we scale and especially the size on which we deployed. So we came down with a list of different hosting plans that we needed, the list of apps that would need to be moved and on which hosting plan they currently were.

That list went to 20 web apps to move. The portal wasn't going to cut it. It was time to bring in the big guns.

### Powershell

Powershell is the Command Line for Windows. It's powered by awesomeness and cats riding unicorns. It allows you to do thing like remote control Azure, import/export CSV files and so much more.

CSV and Azure is what I needed. Since we built a list of web apps to migrate in Excel, CSV was the way to go.

### The Code or rather, The Script

What follows is what is being used. It's heavily inspired of what was found online.

My CSV file has 3 columns: App, ServicePlanSource and ServicePlanDestination. Only two are used for the actual command. I could have made this command more generic but since I was working with apps in EastUS only, well... I didn't need more.

This script should be considered as &quot;Works on my machine&quot;. Haven't tested all the edge cases.

    Param(
        [Parameter(Mandatory=$True)]
        [string]$filename
    )

    Switch-AzureMode AzureResourceManager
    $rgn = 'Default-Web-EastUS'

    $allAppsToMigrate = Import-Csv $filename
    foreach($app in $allAppsToMigrate)
    {
        if($app.ServicePlanSource -ne $app.ServicePlanDestination)
        {
            $appName = $app.App
    		    $source = $app.ServicePlanSource
    		    $dest = $app.ServicePlanDestination
            $res = Get-AzureResource -Name $appName -ResourceGroupName $rgn -ResourceType Microsoft.Web/sites -ApiVersion 2014-04-01
            $prop = @{ 'serverFarm' = $dest}
            $res = Set-AzureResource -Name $appName -ResourceGroupName $rgn -ResourceType Microsoft.Web/sites -ApiVersion 2014-04-01 -PropertyObject $prop
            Write-Host "Moved $appName from $source to $dest"
        }
    }
        