---
title : "1-Click Deployment of Cognitive Services with ARM Templates"
date: 2020-04-04 12:00:00
tags: [azure, deployment, aiml]
---

<iframe width="560" height="315" src="https://www.youtube.com/embed/rKpMC9c7oEs" frameborder="0" allow="accelerometer; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

I've been using Cognitive Services for a while now. I'm creating AI services for hackathons, personal projects, or actual work.

One of the scenarios I'm always facing is to create the service initially. It requires you first to find the right service, then chooses a region, then a pricing tier. Could all of this be made simpler? I think so!

If, like me, you are only using the free tier and are trying to publish into an EastUS/WestUS region, then I'm ready to show you how to do this!

There is a feature in Azure that allows us to create templates for resources. They are called Azure Resource Manager Templates. They are also known as ARM Templates. At a bare minimum, those templates are a combination of a template file and a parameter file.

The template file defines the resources that we need to create while the parameters allow you to set variables at deployment time. You can use the template directly with the Azure Portal and fill the parameters there. Alternatively, you can create a set of parameters in a file and deploy those resources with a Command-Line. Isn't that powerful?

The best use for us at the moment, however, is to reduce the number of choices. We're going to build a single-click deployment option for us to use later on.

Let me show you the repository I created for [One-Click Cognitive Services deployment](https://github.com/MaximRouiller/OneClickCognitiveServices).

Let's take a look at our templates to identify how we are deploying them.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "baseName": {
            "type": "string",
            "defaultValue": "computervision"
        },
        "location": {
            "type": "string",
            "defaultValue": "eastus",
            "allowedValues": ["eastus", "westus"]
        },
        "apiType": {
            "type": "string",
            "defaultValue": "ComputerVision",
            "allowedValues":["ComputerVision"]
        },
        "sku": {
            "type": "string",
            "defaultValue": "F0"
        }
    },
    "resources": [
        {
            "apiVersion": "2017-04-18",
            "name": "[concat(parameters('baseName'), uniqueString(resourceGroup().id))]",
            "location": "[parameters('location')]",
            "type": "Microsoft.CognitiveServices/accounts",
            "kind": "[parameters('apiType')]",
            "sku": {
                "name": "[parameters('sku')]"
            },
            "properties": {
                "customSubDomainName": "[concat(parameters('baseName'), uniqueString(resourceGroup().id))]"
            }
        }
    ]
}
```

There's a lot to unpack here. There are 3 sections in that file: `parameters`, `variables`, and `resources`. We're not going to use the `variables`, and `parameters` is self-describing. Let's look at `resources`.

First, it's an array, which means that we are can create multiple resources at the same time. For the resource we selected, Computer Vision, let's look at the resource structure.  

Excluding whitespaces and sub-properties, there are only around seven meaningful lines in there. Let's go through them.

`apiVersion` is representing the version of the resource provider we use internally. It affects the available properties and schema.

`name` is representing your resource name. Here, we are building a unique name using the `uniqueString` function to ensure we're not creating duplicates within a same Resource Group.

`location` is representing the deployed region of the service. From the default values, we are only letting EastUS and WestUS as options. For a complete list of locations, here's the [reference page](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&WT.mc_id=aiapril-blog-marouill).

`type` is representing the actual service we are deploying. For Cognitive Services, it's all the same type. The attribute `kind` specify which service we want.

`kind` is representing what kind of cognitive services we want to deploy. There are many available and is listed by running the Azure CLI command [`az cognitiveservices account list-kinds -o table`](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest&WT.mc_id=aiapril-blog-marouill#az-cognitiveservices-account-list-kinds)

`sku` is representing the amount we want to pay. We are selecting F0 here by default since we want the free option.

`customSubDomainName` represents the domain name we can enable for token authentication. 

With that, we can deploy cognitive services with the click of a button within any subscription on Azure.

If you are interested in a follow-up article, please let me know in the comments or [on Twitter](https://twitter.com/MaximRouiller).