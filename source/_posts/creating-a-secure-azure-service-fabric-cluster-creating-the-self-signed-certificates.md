---
title : "Creating a secure Azure Service Fabric Cluster - Creating the self-signed certificates"
date: 2017-10-10 07:56:50
tags: [service fabric, keyvault, azure]
---

Service Fabric is an amazing tools that will allow you to create highly resilient and scalable services. However, creating an instance is Azure is not an easy feat if you're not ready for the journey. 

This post is a pre-post to getting ready with Service Fabric on Azure. This is what you need to do before you even start clicking `Create new resource`.

Let's get started.

## Step 0 - Requirements (aka get the tooling ready)

You'll need to install the following:

* [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest&WT.mc_id=maximerouiller-blog-marouill)

## Step 1 - Explaining Service Fabric Explorer Authentication

Service Fabric will require one of many ways of authentication. The default one is to specify a certificate. You could also go with client certificates or Azure Active Directory but it would require us to run additional commands and explain other concepts.

Let's keep this simple. 

We need a certificate to create our cluster. Easy right? Well, not quite if you're looking at the docs. The certificate need to be uploaded into a KeyVault and you need to create it *BEFORE* even trying to create a Secure Service Fabric Cluster.

## Step 2 - Creating a KeyVault instance

> You may need to run `az login` before going further. Ensure that your default subscription is set properly by executing `az account set --subscription <Name|Id>`.

Creating a KeyVault will require a Resource Group. So let's create both right away.

```powershell
# Create our resource group
az group create --location eastus --name rgServiceFabricCluster

# Create our KeyVault Standard instance
az keyvault create --name my-sfcluster-keyvault --location eastus --resource-group rgServiceFabricCluster --enabled-for-deployment
```

Alright! This should take literally less than 20 seconds. We have a KeyVault! We're ready now. Right? Sadly no. We still need a certificate.

## Step 3 - Creating a self-signed certificate into KeyVault

Now is where the everyone gets it wrong. Everyone will tell you how to generate your own certificate (don't mix Windows, Linux, OSX) and how to upload it.

You see, I'm a man with simple taste. I like the little thing in life. Especially in the CLI.

```powershell
# This command export the policy on file. 
az keyvault certificate get-default-policy > defaultpolicy.json

# !IMPORTANT! 
# By default, PowerShell encode files in UTF-16LE. Azure CLI 2.0 doesn't support it at the time of this writing. So I can't use the file directly. 
# I need to tell PowerShell to convert to a specific encoding (utf8).
$policy = Get-Content .\defaultpolicy.json
$policy | Out-File -Encoding utf8 -FilePath .\defaultpolicy.json

# This command creates a self-signed certificate.
az keyvault certificate create --vault-name my-sfcluster-keyvault -n sfcert -p `@defaultpolicy.json
```

Since the certificate was created, we'll need to download it locally and add it to our Certificate Store so that we may login to our Service Fabric Cluster.

## Step 4 - Download the certificate

This will download the certificate to your machine.

```powershell
az keyvault secret download --vault-name my-sfcluster-keyvault -n sfcert -e base64 -f sfcert.pfx
```

You now have the file `sfcert.pfx` on your local machine.

## Step 5 - Installing the certificate locally (Windows only)


```powershell
# This import the certificate in the Current User's certificate store.
Import-PfxCertificate .\sfcert.pfx -CertStoreLocation Cert:\CurrentUser\My\
```

It should show you something along those lines:

```none
> Import-PfxCertificate .\sfcert.pfx -CertStoreLocation Cert:\CurrentUser\My\

   PSParentPath: Microsoft.PowerShell.Security\Certificate::CurrentUser\My

Thumbprint                                Subject
----------                                -------
FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF  CN=CLIGetDefaultPolicy

```

## Final step (6) - Retrieving the necessary options before provisioning

Provisiong a secure cluster requires 3 values in the following order.

1. Resource Id of our KeyVault

```powershell
$resourceId=az keyvault show -n my-sfcluster-keyvault --query id -o tsv
```

2. Certificate URL

```powershell
$certificateUrl=az keyvault certificate show --vault-name my-sfcluster-keyvault -n sfcert --query sid -o tsv
```

3. Certificate thumbprint

We got it in our previosu step but just in case you missed it? Here's how to find it.

```powershell
# Read locally
Get-ChildItem Cert:\CurrentUser\My\

# Read from Azure. Both should match
$thumbprint=az keyvault certificate show --vault-name my-sfcluster-keyvault -n sfcert --query x509ThumbprintHex -o tsv
```

Take those 3 values and get ready to set your Service Fabric Cluster on the Azure Portal.

```powershell
@{Thumbprint=$thumbprint; ResourceId=$resourceId; CertificateUrl=$certificateUrl}
```

## Complete Script

[Source](https://gist.github.com/MaximRouiller/6e5e2b9e0c1c701b9bbbc77342cebe68#file-setupcertificates-ps1)

<script src="https://gist.github.com/MaximRouiller/6e5e2b9e0c1c701b9bbbc77342cebe68.js"></script>

## Follow-up?

Are you interested in automating the creation of an Azure Service Fabric Cluster? What about Continuous Integration of an Azure Service Fabric Cluster?

Let me know in the comments!