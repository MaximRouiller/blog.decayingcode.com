---
title : "HttpRequestException with git pull from GitHub"
date: 2018-03-29 09:00:00
tags: [security, git]
---

I'm working on a Windows machine and some times ago, this error started happening when I did any `git pull` or `git push` operations.

```none
fatal: HttpRequestException encountered.
   An error occurred while sending the request.
Already up-to-date.
```

Okay, we have an HttpException. First, let's be clear that the whole concept of *Exceptions* do not exist in git. This is a .NET concept so it's definitely coming from my [Windows Credential Manager](https://github.com/Microsoft/Git-Credential-Manager-for-Windows).

To enable tracing, you have to set the `GCM_TRACE` environment variable to `1`.

```bash
SET GCM_TRACE=1
```

```powershell
$env:GCM_TRACE = 1
```

Then, I did my `git pull` again.

```none
C:\git\myrepo [master ≡]> git pull
08:59:28.015710 ...\Common.cs:524       trace: [Main] git-credential-manager (v1.12.0) 'get'
08:59:28.441707 ...\Where.cs:239        trace: [FindGitInstallations] found 1 Git installation(s).
08:59:28.459707 ...Configuration.cs:405 trace: [LoadGitConfiguration] git All config read, 27 entries.
08:59:28.466706 ...\Where.cs:239        trace: [FindGitInstallations] found 1 Git installation(s).
08:59:28.473711 ...Configuration.cs:405 trace: [LoadGitConfiguration] git All config read, 27 entries.
08:59:28.602709 ...\Common.cs:74        trace: [CreateAuthentication] detecting authority type for 'https://github.com/'.
08:59:28.684719 ...uthentication.cs:134 trace: [GetAuthentication] created GitHub authentication for 'https://github.com/'.
08:59:28.719709 ...\Common.cs:139       trace: [CreateAuthentication] authority for 'https://github.com/' is GitHub.
08:59:28.745709 ...seSecureStore.cs:134 trace: [ReadCredentials] credentials for 'git:https://github.com' read from store.
08:59:28.748709 ...uthentication.cs:163 trace: [GetCredentials] credentials for 'https://github.com/' found.
08:59:29.183239 ...\Program.cs:422      trace: [Run] System.Net.Http.HttpRequestException: An error occurred while sending the request. ---> System.Net.WebException: The request was aborted: Could not create SSL/TLS secure channel.
   at System.Net.HttpWebRequest.EndGetResponse(IAsyncResult asyncResult)
   at System.Net.Http.HttpClientHandler.GetResponseCallback(IAsyncResult ar)

<snip>
```

Now, we can see that we `Could not create SSL/TLS secure channel`. Also, we can see that my credential manager is version `1.12.0`.

This tells me that something changed somewhere and that the version of my credential manager is probably not up to date. So time to head to the [Windows Credential Manager Release Page](https://github.com/Microsoft/Git-Credential-Manager-for-Windows/releases).

![Windows Credential Manager Release Page](/posts/files/git-cred-manager/release.png)

Alright, so I'm a few versions behind. Let's update to the latest version.

Now, let run another `git pull`.

```none
C:\git\myrepo [master ≡]> git pull
Already up-to-date.
```

Alright so my problem is fixed!

### Why?

Updating git credential manager to the latest version is definitely solving my problem but why did we have that problem in the first place?

If we look at release `1.14.0` we would see something very interesting among the release notes.

> Added support for TLS 1.2 (as TLS 1.0 is being retired).

By doing a bit of search, I ended up on [this blog post](https://githubengineering.com/crypto-deprecation-notice/) by GitHub Engineering which is a depreciation notice for TLS 1.0 since February 1st. 

That's it! Keep your tools updated folks!