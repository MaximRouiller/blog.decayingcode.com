---
title: "Xml Serialization Made Easy"
date: 2009-01-08 17:30:00
tags: []
---

Most of the people who I've seen dealing with XML have some different approaches in dealing with the content.

If you want to read XML content, you normally have many way to go. Some people use XPath to retrieve the value with an XmlDocument object to retrieve what they want. Others will browse by nodes and get to what they want.

Do you see the main problem here? The problem is not that you won't be able to read your information. The problem is the format as well as a lot of "navigation" code to get the information. The easy way to get away with conversions and navigation code is to use the XmlSerializer.

```cs
[XmlRoot(ElementName = "Library")]
public class Library
{
    [XmlAttribute("id")]
    public int ID { get; set; }

    [XmlAttribute("name")]
    public string Name { get; set; }

    public string ToXml()
    {
        StringBuilder sb = new StringBuilder();
        XmlSerializer serializer = new XmlSerializer(typeof(Library));
        StringWriter sw = new StringWriter(sb);
        serializer.Serialize(sw, this);
        return sb.ToString();
    }

    public static Library FromXml(string xml)
    {
        StringReader sr = new StringReader(xml);
        XmlSerializer serializer = new XmlSerializer(typeof(Library));
        Library result = serializer.Deserialize(sr) as Library;
        return result;
    }
}
```

This will easily create an XML with one root node with 2 attributes with ID and Name. The XML will also serialize any children with the proper attributes on them. This is an easy way to serialize as well as deserialize XML without having to mess-up with XPath, XmlDocument, node navigation, etc.