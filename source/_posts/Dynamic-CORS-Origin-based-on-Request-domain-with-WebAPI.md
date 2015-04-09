---
title: "Dynamic #CORS Origin based on Request domain with #WebAPI"
date: 2014-02-19 11:30:00
tags: [asp.net,webapi]
---

So here is the situation, I’m currently working on a website that has multiple environment but all share the same base domain.

As an example let’s say we have those:

*   staging.mydomain.com  <li>qa.mydomain.com  <li>dev.mydomain.com 

Since I don’t want to share my domains with everyone by adding the CORS allowed origins directly from those, I want to my this dynamic for *.mydomain.com but without the wildcard.

First, you’ll need the [Microsoft.AspNet.WebAPI.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) package on NuGet.

You’ll need to add the following in your WebApiConfig.cs:

```cs
public static void Register(HttpConfiguration config)
{
    config.EnableCors();
}
```

However, that method allow us to set a custom policy. Like so:

```cs
public class MyCorsPolicy : ICorsPolicyProvider
{
    public Task<CorsPolicy> GetCorsPolicyAsync(HttpRequestMessage request, CancellationToken cancellationToken)
    {
        var policy = new CorsPolicy();

        var requestUri = request.RequestUri;
        var authority = requestUri.Authority.ToLowerInvariant();
        if (authority.EndsWith(".mydomain.com") || authority == "mydomain.com")
        {
            // returns a url with scheme, host and port(if different than 80/443) without any path or querystring
            var origin = requestUri.GetComponents(System.UriComponents.SchemeAndServer, System.UriFormat.SafeUnescaped);
            policy.Origins.Add(origin);
        }

        return Task.FromResult(policy);
    }
}
```

So with that default policy, we allow anyone from a subdomain to make request to us without having to store a list of every subdomain that could contact us.

Of course, you could also set allowed methods, headers and such in this policy.

Enjoy!