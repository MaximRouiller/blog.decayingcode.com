---
title : "Shooting yourself in the foot with C# Tasks - ContinueWith"
date: 2016-07-20 09:00
tags: [c#, async]
---

Accidentally swallowing exceptions with C# async `ContinueWith()` is a real possibility.

I've accidentally done the same recently on a project.

The idea was that once a method finished running, I wanted to execute a log of the task that was executed.

So my Controller Action looked something like this:

```csharp
[HttpDelete]
public async Task<HttpResponseMessage> DeleteSomething(int id)
{
    await _repository.DeleteSomething(id)
        .ContinueWith(task => _log.Log(task));
    return Request.CreateResponse(HttpStatusCode.OK);
}
```

The delete would go and run the query against the database and delete some records in Azure Table Storage.

The `Log` method would just read the object resulting from the task completing and finish.

What would happen when `DeleteSomething` throws an exception? `ContinueWith` would get passed the faulted `Task` and if you didn't retrow at that point, it would go on and return an `HTTP 200`.

Wow. That's bad. It's like a highly sophisticated `On Error Resume Next`. Welcome to 1995.

Let's fix this. I'm expecting to run this `Task` only when it succeed. So let's make sure I use the right overload.

```csharp
[HttpDelete]
public async Task<HttpResponseMessage> DeleteSomething(int id)
{
    await _repository.DeleteSomething(id)
        .ContinueWith(task => _log.Log(task), TaskContinuationOptions.OnlyOnRanToCompletion);
    return Request.CreateResponse(HttpStatusCode.OK);
}
```

If I keep doing stupid stuff like this, I might turn this into a series of what **NOT** to do.

If you encountered something similar, please let me know in the comments!
