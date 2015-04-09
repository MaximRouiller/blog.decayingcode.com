---
title: "Temporarily ignore SSL certificate problem in Git under Windows"
date: 2014-11-14 18:59:20
tags: [git]
---

So I've encountered the following issue:
 > fatal: unable to access 'https://myurl/myproject.git/': SSL certificate problem: unable to get local issuer certificate 

Basically, we're working on a local Git Stash project and the certificates changed. While they were working to fix the issues, we had to keep working.

So I know that the server is not compromised (I talked to IT). How do I say "ignore it please"?

### Temporary solution

This is because you know they are going to fix it.

PowerShell code:
```ps
$env:GIT_SSL_NO_VERIFY = "true"
```

CMD code:
```bash
SET GIT_SSL_NO_VERIFY=true
```

This will get you up and running as long as you don’t close the command window. This variable will be reset to nothing as soon as you close it.

### Permanent solution

Fix your certificates. Oh… you mean it’s self signed and you will forever use that one? Install it on all machines.

Seriously. I won’t show you how to permanently ignore certificates. Fix your certificate situation because trusting ALL certificates without caring if they are valid or not is juts plain dangerous. 

Fix it.

NOW.