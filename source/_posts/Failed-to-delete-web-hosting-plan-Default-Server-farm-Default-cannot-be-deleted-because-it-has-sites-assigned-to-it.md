---
title: "Failed to delete web hosting plan Default: Server farm 'Default' cannot be deleted because it has sites assigned to it"
date: 2015-03-31 15:14:28
tags: [azure,powershell]
---

So I had this issue where I was moving web apps between hosting plans. As they were all transferred, I wondered why it refused to delete them with this error message.

After a few click left and right and a lot of wasted time, I found [this blog post](http://blogs.msdn.com/b/waws/archive/2014/11/05/azure-powershell-script-to-list-sites-staging-deployment-slots-for-web-hosting-plans-whp.aspx) that provides a script to help you debug and the exact explanation as to why it doesn't work.

To make things quick, it's all about ["Deployment Slots"](http://azure.microsoft.com/en-us/documentation/articles/web-sites-staged-publishing/). Among other things, they have their own _serverFarm_ setting and they will not change when you change their parents in Powershell (haven't tried by the portal).

Here's a copy of the script from Harikharan Krishnaraju for future references:

```ps
Switch-AzureMode AzureResourceManager
$Resource = Get-AzureResource

foreach ($item in $Resource)
{
	if ($item.ResourceType -Match "Microsoft.Web/sites/slots")
	{
		$plan=(Get-AzureResource -Name $item.Name -ResourceGroupName $item.ResourceGroupName -ResourceType $item.ResourceType -ParentResource $item.ParentResource -ApiVersion 2014-04-01).Properties.webHostingPlan;
		write-host "WebHostingPlan " $plan " under site " $item.ParentResource " for deployment slot " $item.Name ;
	}

	elseif ($item.ResourceType -Match "Microsoft.Web/sites")
	{
		$plan=(Get-AzureResource -Name $item.Name -ResourceGroupName $item.ResourceGroupName -ResourceType $item.ResourceType -ApiVersion 2014-04-01).Properties.webHostingPlan;
		write-host "WebHostingPlan " $plan " under site " $item.Name ;
	}
}
```