---
title: "Build fail only on the build server with delay-signed assemblies"
date: 2009-01-30 16:42:00
tags: [build]
---

```text
[Any CPU/Release] SGEN(0,0): error : Could not load file or assembly '<YOUR ASSEMBLY>' or one of its dependencies. Strong name validation failed. (Exception from HRESULT: 0x8013141A)
```

I got that error from our build machine. It crashed wonderfully and would tell me that an assembly could not be loaded.

After 1 hour of search I finally found the problem.

We are in an environment where we delay-sign all of our assemblies and we are fully signing them on the build server as an "AfterDrop" event. Of course, we add a "Skip Verification" for the public key token we are using so that we can put them inside the GAC.

All of our projects (more than 20) we built exactly that way and I was seeing no way why it would happen. I then decided to look at what depended on this assembly. 1 project and it was the one that was failing inside the "Build Status".

I then found something that was used inside that specific project that was never used anywhere else. Somebody used the tab "Settings" to store application settings. Not a bad choice and a perfectly sound decision but... how can this make the build crash?

Well... it seems like using "Settings" force a call to SGEN.exe and SGEN.exe won't take any partially signed assemblies. It's then that I figured out that our build server didn't had any of those "Skip Verification".

After searching Google for some different terms, I found out under the "Build" tab a way to deactivate the call to SGEN. It's called "Generate Serialization Assembly". By default, the value is "Auto". After settings the value at "Off" for "Release" mode only, the build was fixed and we were happy campers.
