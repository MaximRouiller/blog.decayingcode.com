---
title : "Managed Disk is now in GA - Convert all your VMs now!"
date: 2017-02-09 09:00
tags: [azure, vm]
---

Alright so this is kind of a big deal. This has been covered in the past but since this just hit General Availability, you need to get on this now. Or better, yesterday if you have access to a time machine.

Before Managed disks, you had to [create an Azure Storage Account for each VM to avoid IOPS limits](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-guidance-compute-single-vm#disk-and-storage-recommendations). But this wasn't enough to keep your VMs up and running. You had to also manage [availability sets](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-guidance-compute-single-vm#availability-considerations).

This has led a some people as far away from VM as possible. But if you consider the insane advantages of VM Scale Sets (read: massive scale-out, massive machine specs, etc.), you don't want to avoid this card in your solution deck. You want to embrace it. But if you start embracing VMs, you have to start dealing with the Storage Accounts and the Availability Sets and, let's be honest, it was clunky.

Today, no more waiting. It's generally available and it's time to embrace it.

### Migrating Existing VMs to Managed Disks
*Note this code is taken from the sources bellow.*

To convert single VMs without an availability set
```powershell
# Stop and deallocate the VM
$rg = "MyResourceGroup"
$vm = "MyMachine"
Stop-AzureRmVM -ResourceGroupName $rg -Name $vm -Force

# Convert all disks to Managed Disks
ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rg -VMName $vm
```

To Convert VMs within an availability set:
```powershell
$rgName = 'myResourceGroup'
$avSetName = 'myAvailabilitySet'

$avSet =  Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName

Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Managed

foreach($vmInfo in $avSet.VirtualMachinesReferences)
{
   $vm =  Get-AzureRmVM -ResourceGroupName $rgName | Where-Object {$_.Id -eq $vmInfo.id}
   Stop-AzureRmVM -ResourceGroupName $rgName -Name  $vm.Name -Force
   ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vm.Name

}
```

[Source 1](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-migrate-to-managed-disks)
|
[Source 2](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-convert-unmanaged-to-managed-disks#convert-existing-azure-vms-to-managed-disks-of-the-same-storage-type)

### More links

Here's some more resource that may help you convert your VMs.

If you are using the Azure CLI, they [updated their tooling](https://azure.microsoft.com/en-us/blog/azure-cli-managed-disks/) to allow you to manage the "Managed Disks".

If you rather use C# to manage your application, the [.NET Azure SDK is also up to date](https://azure.microsoft.com/en-us/blog/net-manage-azure-managed-disks/).

Finally, if you want to start playing with VM Scale Sets and Managed Disks, here's a [quick "how to"](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-attached-disks).

Are Managed Disks something that you waited for? Let me know in the comments!
