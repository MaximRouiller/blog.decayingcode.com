---
title : "Cleaning up Azure Resource Manager Deployments in Continuous Integration Scenario"
date: 2016-11-16 13:00:00
tags: [azure, deployment]
---

When deploying with Azure Resource Manager Templates (aka: ARM Templates), provisioning an environment has never been that easy.

It's as simple as providing a JSON file that represent your architecture, another JSON file that contains all the parameters for this architecture and boom. Online you go.

Personally, I hate deploying from Visual Studio for anything but testing. Once you start delivering applications, you want something centralized, sturdy and battle tested. My tool of choice is Visual Studio Team Services. VSTS integrates perfectly with Azure with tasks to Create/Upgrade ARM templates on an Azure Subscription.

Our current setup includes 4 environments and 4-5 developers. One of this environment is a CI/CD environment. Every single check-in that happens in a day will be deployed. So our resource group is also being updated like crazy. Just to give you numbers, 50 deployments in a day hasn't been unheard of.

The problem, is the [Azure Resource Manager Deployment limit][1].

![Azure Resource Manager Deployment Limit](/posts/files/arm/limits-deployments.png)

So ... 800 eh? Let's make a calculation. 20 deployments per day, 20 workable day in a week... 400 deployments per months.

2 months. That's how long before we run into an error when deploying on Azure. I've already raised the issue with one of the developers over at Microsoft but in the mid-time, I need to clear this!

Many ways to do this.

### The Portal

Oh boy... don't even think about it. You'll have to do them one by one. There's nothing to multi-select. And you'll need to do that every month/2 months.

Everything that is repeating itself is worth automating.

### Powershell - Normal way

I've tried running the following command:
```powershell
$resourceGroupName = "MyResourceGroup"
Get-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName | Remove-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName
```

The problem with this is that each deployment is going to be deleted synchronously. You can't delete in batch. With 800 deployments to clean up, it took me hours to delete a few hundreds before my Azure Login Powershell session expired and crashed on me.

### Powershell - The Parallel way

Powershell allows for parallel commands to be run side by side. It run those commands in separate sessions in a separate Powershell process.

When I initially ran this command, I had about 300 deployments to clean on one of my resource group. This, of course, launched 300 `powershell.exe` processes that executed to required commands.

```powershell
$path = ".\profile.json"
Login-AzureRmAccount
Save-AzureRmProfile -Path $path -Force

$resourceGroupName = "MyResourceGroup"
$deployments = Get-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName
$deploymentsToDelete = $deployments | where { $_.Timestamp -lt ((get-date).AddDays(-7)) }

foreach ($deployment in $deploymentsToDelete) {
	Start-Job -ScriptBlock {
		param($resourceGroupName, $deploymentName, $path)
		Select-AzureRmProfile -Path $path
		Remove-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -DeploymentName $deploymentName
	} -ArgumentList @($resourceGroupName, $deployment.DeploymentName, $path) | Out-Null
}

```

Then you have to keep track of them. Did they run? Did they fail? `Get-Job` will return you the list of all jobs launched in this session. The list may be quite extensive. So let's keep track only of those running:

```powershell
(Get-Job -State Running).Length
```

If you want to only see the results of the commands that didn't complete and clear the rest, here's how:

```powershell
# Waits on all jobs to complete
Get-Job | Wait-Job

# Removes the completed one.
Get-Job -State Completed | Remove-Job

# Output results of all jobs
Get-Job | Receive-Job

# Cleanup
Get-Job | Remove-Job
```

### The results?

Instead of taking overnight and failing on over half the operations, it managed to do all of those in a matter of minutes.

The longest run I had on my machine was 15 minutes for about 300 deployments. Always better than multiple hours.

If you create a Powershell that automate this task on a weekly basis, you won't have to wait so long. If you include [Azure Automation][2] in this? You're good for a very long time.



[1]: https://docs.microsoft.com/en-us/azure/azure-subscription-service-limits
[2]: https://azure.microsoft.com/en-us/services/automation/
