---
title: "#Kibana in #IIS – Error loading dashboards with JSON extension"
date: 2014-02-07 07:30:00
tags: [iis]
---

So I’ve found this small problem and it’s been referenced before. So as I was installing Kibana in my local IIS 8, it had problem loading files due to an error in MIME type for files with a JSON extension. 

If you find that you are having the same problem, just put the following Web.config file at the root: 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <staticContent>
            <mimeMap fileExtension=".json" mimeType="application/json" />
        </staticContent>
    </system.webServer>
</configuration>
```
