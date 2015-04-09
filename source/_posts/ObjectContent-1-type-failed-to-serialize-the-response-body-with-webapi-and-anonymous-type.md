---
title: "'ObjectContent`1' type failed to serialize the response body with #webapi and anonymous type"
date: 2014-02-25 11:00:00
tags: [webapi]
---

I received this error at some point today about failed serialization of ObjectContent. At first I didn’t see it since I’m making all of my requests with the header “Accept: application/json”.

Here is the type of method on WebAPI I had:

```cs
public object Get()
{
    return new { someValue = false };
}
```

Pretty easy no? See the problem? Me neither. When I was requesting JSON, I received a nicely formatted object exactly like it is displayed there.

When I did the request through the browser, it failed. So I launched Fiddler and try to debug and… it’s basically as soon as you request XML.

So I hit Google (my preferred tool for search Stackoverflow) and I [found this](http://stackoverflow.com/questions/14962134/returning-an-anonymous-type-from-mvc-4-web-api-fails-with-a-serialization-error). This brought me eventually to [this link](http://www.asp.net/web-api/overview/formats-and-model-binding/json-and-xml-serialization#json_anon).

It contains a very similar code base than mine. Object as return type and we return an anonymous type. Let me bring in the quote for you:

> The XML serializer does not support anonymous types or JObject instances. If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.

### Conclusion

Try all formatters on all endpoint before actually shipping to production. In the mid-time, I resolved this by creating a simple nested class that has my value.

JSON.NET had no problems doing the job but the XmlSerializer didn’t like it one bit.