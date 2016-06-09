So I recently had to integrate AppInsights in an AngularJS application. 

The issue I faced was that I didn't want to pre-create my AppInsights instance and instead, rely on Azure Resource Manager to automatically instantiate them. In my [previous post][1], I showed you how to move the Instrumentation Key directly into AppSettings. How do you get it on the client?

You could use MVC and render it directly. But thing is, the application's architecture is that... there's no C# running on this side of the project. No .NET at all. So how do I keep my dependencies low while still taking the AppSettings to the client?

### Http Handlers

Http Handlers was first introduced at the very beginning of .NET. They are light weight, have no dependencies and are very fast. 

Exactly what we need for our basic need.

Here's the code that I used:

```csharp
public class InstrumentationKeyHandler : IHttpHandler
{
    public bool IsReusable => true;

    public void ProcessRequest(HttpContext context)
    {
        var setting = ConfigurationManager.AppSettings["InstrumentationKey"];
        context.Response.Clear();
        context.Response.ContentType = "application/javascript";
        context.Response.Write($"(function(){{window.InstrumentationKey = \'{setting}\'}})()");
    }
}
```
Here's how you configure it in your `web.config`:

```xml
<?xml version="1.0"?>
<configuration>
    <system.webServer>
        <handlers>
            <add name="InstrumentationKey" type="MyNamespace.InstrumentationKeyHandler, MyAssembly" resourceType="Unspecified" path="InstrumentationKey.ashx" verb="GET" />
        </handlers>
    </system.webServer>
</configuration>
```

And how you use it :

```javascript
<script src="InstrumentationKey.ashx"></script>
<script type="text/javascript">
    if (window.InstrumentationKey) {
        console.debug('InstrumentationKey found.');
        //TODO: Insert AppInsights code here.
    } else {
        console.debug('InstrumentationKey missing.')
    }
</script>
```

### Why does it work?

We are relying on the browser's basic functionality when loading scripts. All scripts will be downloaded asynchronously initially but they will always be ran sequentially.

In this scenario, we pre-load our Instrumentation Key inside a global variable and use it in the next script tag. 

[1]: /post/importing-your-appsinsights-instrumentationkey-into-your-appsettings/
