---
title : "Enable Transparent Data Encryption on SQL Azure Database"
date: 2017-01-31 10:00:00
tags: [azure, sql]
---

Among the many recommendations to make your data secure on Azure, one is to implement Transparent Data Encryption.

Most of the ways you'll see online to enable it is to run the following command in SQL:

```sql
-- Enable encryption  
ALTER DATABASE [MyDatabase] SET ENCRYPTION ON;  
GO  
```

While this may be perfectly valid for existing database, what if you want to create it right from the start with TDE enabled?

That's where ARM templates normally come in. It's also where the documentation either fall short or isn't meant to be used as-is right now.

So let me give you the necessary bits for you to enable it.

### Enabling Transparent Data Encryption

First, create a new array of sub resources for your **database**. Not your server. Your database. It's important otherwise it just won't work.

Next, is to create a resource of type `transparentDataEncryption` and assign the proper properties.

It should look like this in your JSON Outline view.

![Enabling Transparent Data Encryption on Azure](/posts/files/enable-tde.png)

I've included the database ARM template I use for you to copy/paste.

### ARM Template

```json
{
 "name": "[variables('databaseName')]",
 "type": "databases",
 "location": "[resourceGroup().location]",
 "tags": {
   "displayName": "Database"
 },
 "apiVersion": "2014-04-01-preview",
 "dependsOn": [
   "[concat('Microsoft.Sql/servers/', variables('sqlserverName'))]"
 ],
 "properties": {
   "edition": "[parameters('edition')]",
   "collation": "[parameters('collation')]",
   "maxSizeBytes": "[parameters('maxSizeBytes')]",
   "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
 },
 "resources": [
   {
     "name": "current",
     "type": "transparentDataEncryption",
     "dependsOn": [
       "[variables('databaseName')]"
     ],
     "location": "[resourceGroup().location]",
     "tags": {
       "displayName": "Transparent Data Encryption"
     },
     "apiVersion": "2014-04-01",
     "properties": {
       "status": "Enabled"
     }
   }
 ]
}
```

### Want more?

If you are interested in more ways to secure your data or your application in Azure, please let me know in the comments!
