---
title : "Wrapping Node.js Azure Table Storage API to enable async/await"
date: 2019-03-14 15:30:00
tags: [nodejs, storage, azure]
---

I love the latest and greatest. Writing code by using the new language syntax is fantastic.

What happens when your favorite library doesn't? You are stuck trying to find a workaround. We all hate workarounds, but they are the glue that keeps our code together at the end of the day.

Between the runtime, the frameworks, the libraries, and everything else... we need everyone on the same page.

Recently, I had to use [Azure Node.js Table Storage API](http://azure.github.io/azure-storage-node/#toc6__anchor).

First, let me say *I know*. Yes, there is a v10 that exist, and this is the v2. No, [v10 doesn't support Table Storage yet](https://github.com/Azure/azure-storage-js#azure-storage-sdk-v10-for-javascript). So, let's move on.

Here's the code I wanted to see:

```javascript
let storage = require('azure-storage');
// ...

async function getAllFromTable() {
  let tableService = storage.createTableService(connectionString);
  let query = new storage.TableQuery()
    .where('PartitionKey eq ?', defaultPartitionKey);
  return await queryEntities(tableService, 'TableName', query, null);
}
```

Here's the code that I had:

```javascript
async function getAllFromTable() {
  return new Promise((resolve, reject) => {
    let tableService = storage.createTableService(connectionString);
    let query = new storage.TableQuery()
      .where('PartitionKey eq ?', defaultPartitionKey);

    tableService.queryEntities('TableName', query, null, function (err, result) {
      if (err) {
        reject(err);
      } else {
        resolve(result);
      }
    });
  });
}
```

The sight of function callbacks gave me flashbacks to a time where code indentation warranted wider monitors.

## The workaround

Here's the temporary workaround that I have for now. It allows me to wrap highly used functions into something more straightforward.

```javascript
async function queryEntities(tableService, ...args) {
    return new Promise((resolve, reject) => {
        let promiseHandling = (err, result) => {
            if (err) {
                reject(err);
            } else {
                resolve(result);
            }
        };
        args.push(promiseHandling);
        tableService.queryEntities.apply(tableService, args);
    });
};
```

### Overview of the workaround

1. We're using async everywhere
2. Using Rest parameters `args` allow us to trap all parameters from that API
3. We're wrapping the proper promise and inserting it into the arguments
4. We're calling the relevant API with the proper argument.

## Conclusion

That's it. While the Node.js Storage v10 is having table storage implemented, I recommend wrapping table storage code into a similar structure.

This will allow you to use the new language syntax while they update the library.