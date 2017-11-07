---
title : "Microsoft Azure Dashboards quick tips"
date: 2017-01-06 10:00:00
tags: [azure]
---

Dashboards are a fantastic way of monitoring applications. However, most people don't really know how to benefit from them.

All screenshots are from when you view your dashboard in **Edit Mode** (after clicking **Edit Dashboard**)

### Markdown tiles

Those are readily accessible under `General` in the Tile Gallery blade.

![Markdown tile](/posts/files/azure-dashboards/markdown-tile.png)

Drag and dropping will allow you to set a `Title`, `Subtitle` as well as content.

This fits perfectly well with the next feature.

### Deep linking resources

Let's say I want to link to an AppService webjobs but I don't want to have the whole widget. Or maybe I just want to see the deployment list. Not just the active one.

You could spend the five minutes to search and add the widget which will increase the load time or... you could just create a link to it.

Want to direct-link to an AppService console?

`https://portal.azure.com/#resource/subscriptions/{Subscription_Id}/resourceGroups/{Resource_Group_Name}/providers/Microsoft.Web/sites/{Site_Name}/console`

Want to direct-link to the AppService Deployment Options?

`https://portal.azure.com/#resource/subscriptions/{Subscription_Id}/resourceGroups/{Resource_Group_Name}/providers/Microsoft.Web/sites/{Site_Name}/DeploymentSource`

Basically, just navigate to where you want to go, copy the link and you are almost done.

Use the previous trick to create a markdown tile and add links to your desired locations and you now have instant navigation to the exact feature you want to manage.

### Sharing Dashboards

Now that you made that awesome dashboard full of custom links and useful widgets... what about sharing it so others don't have to build one themselves?

It is possible to Share a dashboard by saving them through a resource group. By default, it will try to save them in a `dashboards` resource group but they can also be saved within existing resource group.

This allows you to easily share carefully crafted dashboards with other members of the team.

This can be accessed with a link right beside the `Edit Dashboard` button.

If you are interested in deploying templates with ARM in the future, I would keep an eye on [this issue](https://github.com/Azure/azure-resource-manager-schemas/issues/144) on GitHub.

If you want to try the undocumented way (which may break in the future), check out [this blog post](http://wp.sjkp.dk/custom-azure-portal-dashboard-with-arm-templates/).

If you want to *UnShare* a dashboard, just navigate to the dashboard and click *Share* again and press on `UnPublish`.

### Do you want more?

Is that a subject that you would be interested in? Do you want more tips and tricks about the Azure Portal and useful dashboard configurations?

Please let me know in the comments!
