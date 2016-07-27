---
title: "Logging detailed DbEntityValidationException to AppInsights"
date: "2016-06-14 10:00:00"
tags: [azure,appinsights]
---

I've been having some issue logging exception to AppInsights when `DbEntityValidationException` were thrown.

AppInsight would show me the exception with all associated details, but would not show me what validations were faulty.

It just happens that the exception will not be deserialized. They will take the Message, the StackTrace and a few other default properties, but that's it.

So how do I get the content of `EntityValidationErrors`? Manually of course!

### Retrieving validation errors

In my scenario, I can do a simple SelectMany since I know I'm dealing with just one entity at a time. Depending on your scenario, you should consider inspecting the `Entity` property instead of just using the `ValidationErrors`.

Here's what I did:

```csharp
var telemetryClient = new TelemetryClient();
try
{
    // do stuff ...
}
catch(DbEntityValidationException ex)
{
    Dictionary<string, string> properties = ex.EntityValidationErrors.SelectMany(x => x.ValidationErrors)
                        .ToDictionary(x => x.PropertyName, x => x.ErrorMessage);
    telemetryClient.TrackException(ex, properties);
}
```

Here's how to handle many entities:

```csharp
foreach (var validationError in dbException.EntityValidationErrors)
{
    var properties = validationError.ValidationErrors.ToDictionary(x => x.PropertyName,
        x => x.ErrorMessage);
    properties.Add("_EntityType", validationError.Entry.Entity.GetType().FullName);
    telemetryClient.TrackException(ex, properties);
}
```

The only caveat would be that the exception would be logged as many time as you have invalid entities.

On that, back to tracking more exceptions!
