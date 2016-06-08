---
title: "Importing your AppInsights Instrumentation Key directly into your AppSettings"
date: 2016-06-07 15:00
tags: [azure, appinsights]
---
So I had an application recently that needed to use AppInsights. The application was deployed using Visual Studio Release Management so everything needed to be deployed using the Azure Resource Management template.

### Problem

You can't specify an instrumentation key. It is created automatically upon creation.

### Solution

Retrieve the key directly from within the template.

Here's a trimmed down version of an ARM template that does exactly this. 

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...
  },
  "variables": {
    ...
  },
  "resources": [    
    {
      "name": "[variables('WebAppName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [...],
      "tags": { ...},
      "properties": { ... },
      "resources": [
        {
          "name": "appsettings",
          "type": "config",
          "apiVersion": "2015-08-01",
          "dependsOn": [...],
          "tags": {...},
          "properties": {
            "InstrumentationKey": "[reference(resourceId('Microsoft.Insights/components', variables('appInsightName')), '2014-04-01').InstrumentationKey]"
          }
        }
      ]
    },
    {
      "name": "[variables('appInsightName')]",
      "type": "Microsoft.Insights/components",
      ...
    }
  ]
}

```

### Key piece

See that little AppSettings named `InstrumentationKey`? That's where the magic happens.

Your instrumentation key is now bound to your WebApp AppSettings without carrying magic strings. 
