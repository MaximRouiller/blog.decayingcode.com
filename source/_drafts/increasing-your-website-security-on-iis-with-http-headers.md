---
title: "Increasing your website security on IIS with HTTP headers"
date: 2016-07-14 13:20
tags: [security, iis]
---

### HTTP Strict Transport Security (HSTS)

#### What is it?
HSTS is a policy integrated within your browser that ensure that no protocol downgrade happens.

That means going from HTTPS > HTTP or in the case where the certificate is not valid, disallowing the page to load all together.

So let's say I type `http://www.securewebsite.com` in my address bar, the browser will automatically replace `http://` by `https://`.

Let's say now that the certificate was replaced by a [man in the middle]() attack, it will simply show an error page without allowing you to skip that page.

#### How do I implement it?

This consist in sending the header `Strict-Transport-Security` with a `max-age` value in seconds.

This would enforce the policy for 1 year: `Strict-Transport-Security: max-age=31536000; includeSubdomains; preload`

With IIS and its `web.config`, we force HTTPS urls and we automatically flag the request with the right header.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <rewrite>
            <rules>
                <rule name="HTTP to HTTPS redirect" stopProcessing="true">
                    <match url="(.*)" />
                    <conditions>
                        <add input="{HTTPS}" pattern="off" ignoreCase="true" />
                    </conditions>
                    <action type="Redirect" url="https://{HTTP_HOST}/{R:1}"
                        redirectType="Permanent" />
                </rule>
            </rules>
            <outboundRules>
                <rule name="Add Strict-Transport-Security when HTTPS" enabled="true">
                    <match serverVariable="RESPONSE_Strict_Transport_Security"
                        pattern=".*" />
                    <conditions>
                        <add input="{HTTPS}" pattern="on" ignoreCase="true" />
                    </conditions>
                    <action type="Rewrite" value="max-age=31536000; includeSubdomains; preload" />
                </rule>
            </outboundRules>
        </rewrite>
    </system.webServer>
</configuration>
```

#### Limits

I could talk about the limits but [Troy Hunt](https://www.troyhunt.com/understanding-http-strict-transport/) does an infinitely better job of explaining it than I do.

Also, please be aware that all latest versions of any modern browsers support this. However, IE10 and less will not protect you from these types of attacks.

IE11 and edge are implementing this security feature.

### X-Frame-Options

What a user is trying to display your website within an `iframe`? Most of the time, this is not a desired nor a tested scenario. At worse, it's just another attack vector.

Let's block it.

#### What is it?

X-Frame-Options allow you to set whether a domain is allowed or not to display your content within an `iframe`. Since nobody uses this technology today, we can either disable it or restrict it to the same domain as the requesting site.

It protects you from attacks called [clickjacking](https://www.owasp.org/index.php/Clickjacking).

#### How do I implement it?

```none
X-Frame-Options: [deny|sameorigin]
```

If you are using IIS, you can simply include this in its configuration.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <httpProtocol>
            <customHeaders>
                <add name="X-Frame-Options" value="DENY" />
            </customHeaders>
        </httpProtocol>
    </system.webServer>
</configuration>
```

Please note that deny will simply disallow any iframe from being used whether they come from your site, a subdomain or anywhere. If you want to allow *same origin* iframes, you have to replace `DENY` by `sameorigin`.

### X-XSS-Protection

Certain browsers have a security mechanism that detects when a [XSS attack](https://www.owasp.org/index.php/Cross-site_Scripting_(XSS)) is trying to take place.

When that happens, we want the page to be blocked and to not sanitize the content.

### What is it?

This is a security feature that was first built within IE8. It was then brought into all Webkit browsers (Chrome & Safari). Each have their own criteria about what is an XSS attack but each will use that header to activate/deactivate/configure that option.

#### How do I implement it?

```none
X-XSS-Protection: 1; mode=block
```

If you are using IIS, you can simply include this in its configuration.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <httpProtocol>
            <customHeaders>
                <add name="X-XSS-Protection" value="1; mode=block" />
            </customHeaders>
        </httpProtocol>
    </system.webServer>
</configuration>
```

### Content-Security-Policy

Let's say I'm using a library in my project. This sprint, we update this library to the latest version, test it and everything seems to work. We push it to production and bam. Suddenly, our users are being compromised. What you didn't know is that plugin was compromised a month ago. It loaded an external script and ran its script like it was coming from your own website.

#### What is it?

This header prevents most Cross Site Scripting attacks by controlling from where script, css, plugins, etc. can actually be run from.

#### How do I implement it?

This one requires careful tweaking. There is literally [tons](https://content-security-policy.com/) of options to define it.

The most basic setting you can do is this:

```none
Content-Security-Policy: script-src 'self;'
```

This will restrict all JavaScript files to only come from your own domain. If you are using Google Analytics, you would need to add that domain. Like so:

```none
Content-Security-Policy: script-src 'self www.google-analytics.com ajax.googleapis.com;'
```

A good default to start with?
```none
Content-Security-Policy: default-src 'none'; script-src 'self'; connect-src 'self'; img-src 'self'; style-src 'self';
```
From that point, test your site with the console open. Check which domains are being blocked, and white list them. You will see messages like these in the console:

> Refused to load the script 'http://...' because it violates the following Content Security Policy directive: "...".

As always, here's the IIS version to implement it with the `.config` file.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <httpProtocol>
            <customHeaders>
                <add name="Content-Security-Policy" value="default-src 'none'; script-src 'self'; connect-src 'self'; img-src 'self'; style-src 'self';" />
            </customHeaders>
        </httpProtocol>
    </system.webServer>
</configuration>
```
