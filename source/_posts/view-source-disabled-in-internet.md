---
title: "View Source disabled in Internet Explorer?"
date: 2010-03-03 17:46:00
tags: []
---

Found the fix after scouting the forums.

The main reason is because the caching of SSL pages are disabled and are not on disk. Somehow, IE doesn't allow you to view the source of those pages.

To fix the issue, open the registry and go to the following key:

**HKCU\Software\Microsoft\Windows\CurrentVersion\InternetSettings\**

There should be a REG_DWORD value with the name "DisableCachingOfSSLPages". The value should be set to "0x00000001". Change it to "0x00000000" and restart Internet Explorer.

This should allow you to view the HTML of your SSL pages when working in a secure environment.

