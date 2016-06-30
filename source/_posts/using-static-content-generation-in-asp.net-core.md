---
title: "Using Static Content Generation in ASP.NET Core"
date: 2016-06-30 13:00
tags: [asp.net core, middleware, static content, azure]
---

There are not a lot of tool in ASP.NET that allows you to generate static content on a site. The only good one out there is [Wyam][1] written by [Dave Glick][2].

Others are excellent but they have the problem of running on NodeJS. While I don't have a problem with node, lots of other .NET developers do. Most of them don't write JavaScript for a living and would just like to use the latest tech to get things done.

So I came up with a proof of concept.

### What I want to do?

I want to be able to create static files for a website without having to run the ASP.NET MVC pipeline every time. I'm talking here about content that doesn't change very much.

From there, we'll ensure that ASP.NET Core can serve those static files without having to resort to running MVC.

Can it be done? Have I gone mad? The answer is yes.

### ASP.NET Core Architecture

The architecture of ASP.NET Core allow us to be at any time, part of the pipeline of the content generation.

One of this piece of this architecture is a Middleware. What's a middleware? It's an element of a pipeline that is ran before and after the actual user code. Elements of a pipeline are executed in order and call the next one in the pipeline. This allow us to run pre/post within the same class.

The theory here is, if we are high enough in the pipeline, we can intercept calls after they reach ASP.NET MVC to generate our files but low enough so that Kestrel can still serve our static files.

### Startup.cs

First, we need to ensure that we set our Startup.cs properly. The first middleware are going to check for the default files. If they are found, they will stop the pipeline and just serve the file. If they are not found, they will get our middleware and finally MVC.

```csharp
app.UseDefaultFiles(); // <== this is not present by default. Add it.
app.UseStaticFiles();

// ********* This is where we want to be ********
app.UseMiddleware<StaticGeneratorMiddleware>(); // <=== we'll create this middleware in a minute

app.UseMvc(routes =>
{
    routes.MapRoute(
        name: "default",
        template: "{controller=Home}/{action=Index}/{id?}");
});
```

Here's how it would look like visually.

![Middleware execution order](/posts/files/static-middleware.png)

As the request comes in, each middleware is executed in order and once the bottom of the pipeline is reached, each middleware gets to execute one last time on the way up.


### Creating StaticGeneratorMiddleware

```csharp
public class StaticGeneratorMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IHostingEnvironment _hostingEnvironment;

    public StaticGeneratorMiddleware(RequestDelegate next, IHostingEnvironment hostingEnvironment)
    {
        if (next == null) throw new ArgumentNullException(nameof(next));
        if (hostingEnvironment == null) throw new ArgumentNullException(nameof(hostingEnvironment));

        _next = next;
        _hostingEnvironment = hostingEnvironment;
    }

    public async Task Invoke(HttpContext context)
    {
        if (context == null) throw new ArgumentNullException(nameof(context));
        if (context.Request == null) throw new ArgumentNullException(nameof(context.Request));

        // we skip non-html content for now
        if (context?.Request?.ContentType?.Contains("text/html") == true)
        {
            await _next(context);
            return;
        }

        // we skip the first slash and we reverse the slashes
        var baseUrl = context.Request.Path.Value.Substring(1).Replace("/", "\\");
        // default files will look for "index.html"
        var destinationFile = Path.Combine(_hostingEnvironment.WebRootPath, baseUrl, "index.html");

        EnsureDestinationFolderExist(destinationFile);

        // replace the output stream to collect the result
        var responseStream = context.Response.Body;
        var buffer = new MemoryStream();
        var reader = new StreamReader(buffer);
        context.Response.Body = buffer;

        // execute the rest of the pipeline
        await _next(context);

        // reset the buffer and retrieve the content
        buffer.Seek(0, SeekOrigin.Begin);
        var responseBody = await reader.ReadToEndAsync();

        // output the content to disk
        await WriteBodyToDisk(responseBody, destinationFile);

        // copy back our buffer to the response stream
        buffer.Seek(0, SeekOrigin.Begin);
        await buffer.CopyToAsync(responseStream);
    }

    private void EnsureDestinationFolderExist(string destinationFile)
    {
        var directoryName = Path.GetDirectoryName(destinationFile);
        Directory.CreateDirectory(directoryName);
    }

    private async Task WriteBodyToDisk(string responseBody, string destinationFile)
    {
        using (FileStream fs = new FileStream(destinationFile, FileMode.Create))
        using (StreamWriter sw = new StreamWriter(fs))
        {
            await sw.WriteAsync(responseBody);
        }
    }
}
```

### What does it do?

It will create a folder hierarchy under your `wwwroot` folder and create `index.html` files that match the request URL.

Once the file exist, other request on the same URL will find the `index.html` that was created and skip the whole pipeline.

### Pushing it further?

If we want to go mad scientist here, we could create a crawler that would access the urls on our site or maybe even generate a sitemap and loop on those urls.

What would happen is that everything that was served as HTML by MVC would be located in our `wwwroot` folder. We could then technically take this folder and just put it on any web server nad it would host your web site.

### Pushing it to the cloud

Another step we could take is instead of outputting to disk, we would output to Azure Blob Storage. Immediately, you could spawn any instances of websites and none would access MVC unless the blob storage didn't already exist.

### Revoking files

Another middleware that should be added above `UseDefaultFiles` and `UseStaticFiles` is a middleware that would delete files after certain conditions. Otherwise, we will never regerate those files.

### Conclusion

With a simple middleware, we can generate static content directly from ASP.NET Core and allow it to be served locally without any other plugins.

Is there easier way to go static with ASP.NET Core? Probably.

Am I still mad? Yep and I stand by my crazy code.

[1]: http://wyam.io/
[2]: http://daveaglick.com/
