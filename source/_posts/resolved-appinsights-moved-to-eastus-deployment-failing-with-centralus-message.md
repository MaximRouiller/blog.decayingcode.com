---
title : "Resolved: AppInsights moved to EastUS, deployment failing with CentralUS message"
date: 2016-11-21 13:30
tags: [azure, appinsights]
---

**TLDR** AppInsights were moved to EastUS. AutoScale Settings and Alerts were kept in CentralUS causing a chain-reaction of failure all around. Code to clean-up available at end of post.

As AppInsights hit General Availability at Microsoft Connect 2016, a few issues were introduced that caused our VSTS builds to start failing. Here's the message that we got:

```none
2016-11-18T19:25:33.9545678Z [Azure Resource Manager]Creating resource group deployment with name WebSiteSQLDatabase-20161118-1925
2016-11-18T19:25:37.8711397Z ##[error]The resource 'myresource-int-myresource-int' already exists in location 'centralus' in resource group 'myresource-int'. A resource with the same name cannot be created in location 'East US'. Please select a new resource name.
2016-11-18T19:25:37.9071426Z ##[section]Finishing: Azure Deployment:Create Or Update Resource Group action on eClientRcgt-int
```

So I started debugging. After a few days trying to get this issue fixed, I decided to generate the template from the portal. Looked up the `myresource-int-myresource-int` inside of it and found out that it was an automatically generated name for `Microsoft.insights/autoscalesettings`. The worse was... its location was Central US. And it was not alone.

Other Alert rules were also located in Central US and just fixing the `autoscalesettings` would get me other error messages.

Of course, there's no easy way in the portal to delete those. However, with PowerShell, it's trivial.

It is important to note that it's perfectly safe to delete them on our end since we deploy with Azure Resource Manager templates. They will be recreated at the next CI/CD run.

Here's the quick code to delete them if you encounter this issue.

```powershell
$resource = Get-AzureRmResource
$rgn = 'resourceGroup'
$resource | where { $_.ResourceType -eq 'Microsoft.insights/autoscalesettings' -and $_.Location -eq 'centralus' -and $_.ResourceGroupName -eq $rgn } | Remove-AzureRmResource
$resource | where { $_.ResourceType -eq 'Microsoft.insights/alertrules' -and $_.Location -eq 'centralus' -and $_.ResourceGroupName -eq $rgn } | Remove-AzureRmResource
```
