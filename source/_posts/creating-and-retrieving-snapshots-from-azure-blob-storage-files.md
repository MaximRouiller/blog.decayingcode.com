---
title: "Creating and retrieving snapshots from Azure Blob Storage files"
date: 2016-06-20 09:00:00
tags: [azure,storage]
---

When uploading files to Azure Storage, you might be interested in keeping the version of the file that is already existing.

Are you to download those files locally? Maybe even back them up? Well, yes if they need to be kept for a long time, but another solution is to just make a snapshot of them.

Azure Blob Storage allows you to flag the current state of a blob to allow you to return to it later with all of its content available.

### Pre-requisite

```powershell
Install-Package WindowsAzure.Storage
```

### Creating some random data First
First, let's create some data with which to work.

```csharp
var blobClient = CloudStorageAccount.DevelopmentStorageAccount.CreateCloudBlobClient();
var container = blobClient.GetContainerReference("test");
container.CreateIfNotExists();

var blob = container.GetBlockBlobReference("textfile.txt");
blob.UploadFromFile("MyTextFile.txt");
```

All content can be lost if the file that I upload is uploaded in the same `BlockBlob`.

What if my app require that I version those files?

### Making a snapshot

Making a snapshot is as easy as adding this line after uploading:

```csharp
blob.Snapshot();
```

So now I can run this code and the content of my previous file will still be accessible.

```csharp
var blob = container.GetBlockBlobReference("textfile.txt");
blob.UploadFromFile("ADifferentTextFile.txt");
```
### Retrieving a snapshot

Since more than one snapshot can be taken, we have to retrieve them as a list.

Here's how to query for all snapshots of a specific file and output its content directly on screen.

```csharp
IEnumerable<CloudBlockBlob> snapshots = container.ListBlobs(blobListingDetails: BlobListingDetails.Snapshots, useFlatBlobListing:true)
    .Cast<CloudBlockBlob>()
    .Where(x=> x.IsSnapshot && x.Name == "textfile.txt")
    .ToList();

foreach (var item in snapshots)
{
    using (var sr = new StreamReader(item.OpenRead()))
    {
        Console.WriteLine($"Snapshot of {item.Name} taken at date: {item.SnapshotTime.Value.ToLocalTime()} :");
        Console.WriteLine(sr.ReadToEnd());
    }
}
```

### Restoring a snapshot over an existing file

What if you just need to do the revert and not bother about the actual content of the file?

You can easily copy a snapshot to its current blob using something along this line:

```csharp
var blob = container.GetBlockBlobReference("textfile.txt");
var wantedSnapshot = snapshots.First(); // or any condition that you wish
blob.StartCopy(wantedSnapshot);
```

### Deleting a blob with snapshots

Once a blob have snapshots, you will not be able to delete it by just calling `blob.DeleteIfExists()`. This is a safety net in case of accidental deletion.

If you are sure that you want to delete a blob with all its snapshot, you will need to use the proper overload.

```csharp
// deletes blob and snapshots
blob.DeleteIfExists(DeleteSnapshotsOption.IncludeSnapshots);

//delete snapshots only
blob.DeleteIfExists(DeleteSnapshotsOption.DeleteSnapshotsOnly);
```

### Considerations

#### Cost

First, snapshots aren't free. You pay for data that you are using. Each snapshot that is equal to the source data is free. After all, nothing changed. If data is changed, only the delta will be taken into consideration when billing you.

However, if data from a `BlockBlob` is changed by using `UploadFile`, `UploadText`, `UploadStream` or `UploadByteArray`, it will cause the whole blob to be considered *changed*. Even if the content is completely equal to the original blob. So be careful.

For more detailed information considering billing checkout this article about [how snapshots accrue charges][1].



[1]: https://msdn.microsoft.com/en-us/library/azure/hh768807.aspx
