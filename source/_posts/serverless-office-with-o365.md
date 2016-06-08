---
title: "Server-less Office with O365"
date: 2016-06-08 8:00:00
tags: [azure,o365]
---
I recently had a very interesting conversation with a colleague of mine about setting up basic services for small/medium businesses. 

His solution was basically to configure a server on-site and offer Exchange, DNS, Active Directory, etc. in a box.

We had some very interesting discussion but let me bring you what I suggested to him to save him time and his customer money.

Note: I'm not an O365 expert. I'm not even an IT Pro. Just a very passionate technologist. 

### Replacing Exchange with O365

First, Exchange is a very complicated beast to configure. Little mistakes are very time consuming to debug.

O365 comes with everything up and running. Short of configuring Quotas and a few other things, it's already up and running. 

But the most important point for someone having a client on O365 is that when there's an issue anywhere, you don't have to get on-site or VPN/RDP to the machine. You go to the online dashboard, you debug and you deliver value.

You save on time and help your client better use its resources. 

### DNS and Active Directory

When creating an O365 service, it basically ships with its own Active Directory. Windows 10 allows you to join your local machine to an Azure Active Directory domain.

No more need for a Domain Controller client-side.

### What is needed?

Buy a Wireless router. Plug it in your client's office. Once all the machines have joined the domain, they can all collaborate with each other.

If somethings goes wrong, replace the router. Everything else is cloud based.

* File Sharing? OneDrive is included
* Email? O365
* Active Directory? On Azure

### Why go that way?

Let's be clear. The job is going to change. Small clients never needed the infrastructure we pushed on them earlier. At best, the server was idle and at worst that server ended up in a closet overheating or being infested by pest (not joking). There just wasn't anything like the Cloud to provide for them. 

By helping them lighten their load, we free up our own time to serve more clients and offer them something different. 

If you are not doing this now, somebody else is going to offer your client that opportunity. After the paperless office, here's the server-less office.
