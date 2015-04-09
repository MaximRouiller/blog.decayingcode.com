---
title: "Curation Tool – Creating unique GUID from a Uri"
date: 2014-01-14 08:50:24
tags: [asp.net]
---

I’m currently building a tool that helps me curate information from the web. Right now, it’s restricted to Twitter however, more will come. 

One of the many problems I’ve faced is to avoid duplicate URLs. The main problem remains that URLs change and they sometime contains junk.

I’ve not resolved any possible scenarios but I would like to show you a code snippet of what I have so far. 

```cs
public static class UrlHelper
{
    public static string GenerateId(Uri url)
    {
        var hashingAlgorithm = MD5.Create();
        var computedHash = hashingAlgorithm.ComputeHash(Encoding.UTF8.GetBytes(url.ToString()));
        return BitConverter.ToString(computedHash).Replace("-", "").ToLowerInvariant();
    }

    public static Uri FilterQueryString(Uri url)
    {
        try
        {
            var builder = new UriBuilder(url);

            var parameters = ExtractParameters(builder.Query);
            var queryString = CreateQueryString(parameters);
            builder.Query = queryString;

            return builder.Uri;
        }
        catch
        {
            return null;
        }
    }

    private static string CreateQueryString(IEnumerable&lt;QueryStringParameter&gt; parameters)
    {
        var aggregate = parameters
            .OrderBy(t =&gt; t.Name)
            .Where(t =&gt; !t.Name.Contains("utm_"))
            .Select(t =&gt; string.Format("{0}={1}", t.Name, t.Value))
            .Aggregate(string.Empty, (current, next) =&gt; current + "&amp;" + next);
        return aggregate;
    }

    private static IEnumerable&lt;QueryStringParameter&gt; ExtractParameters(string query)
    {
        string withoutQuestionMark = query;
        if (withoutQuestionMark.IndexOf("?", StringComparison.Ordinal) == 0)
            withoutQuestionMark = withoutQuestionMark.Remove(0, 1);
        if (!string.IsNullOrWhiteSpace(withoutQuestionMark))
        {
            string[] nameValues = withoutQuestionMark.Split('&amp;');
            foreach (var nameValue in nameValues)
            {
                var pairSplitted = nameValue.Split('=');
                if (pairSplitted.Length == 2)
                    yield return new QueryStringParameter(pairSplitted[0], pairSplitted[1]);
            }
        }
    }
}
```

So what does this code do? It exposes 2 methods. One for filtering and the other one to generate a string id that will look very similar to a GUID. The filter will reorganize the query string by alphabetical order and remove any “utm_*” from the query string. Those parameters are normally used in Google Analytics Campaigns and will only add noise to our data. They can safely be stripped out.

As for generating the ID, I just generate an MD5 hash which I convert into hexadecimal without separators. 

One GUID to go you say? You got it.