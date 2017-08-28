---
title : "git remote code execution bug of August 2017"
date: 2017-08-28 10:00:00
tags: [security, git]
---

This is a TLDR of the git bug.

There was a bug in git that affected the `clone` command.

## What's the bug?

A malformed `git clone ssh://..` command would allow user to insert an executable within the URL and it would execute it.

## Am I affected?

Easiest way to check is to  run this simple command:

```bash
git clone ssh://-oProxyCommand=notepad.exe/ temp
```

Notepad opens? You're vulnerable.

What you want is this:

```none
C:\git_ws> git clone ssh://-oProxyCommand=notepad.exe/ temp
Cloning into 'temp'...
fatal: strange hostname '-oProxyCommand=notepad.exe' blocked
```

### Visual Studio 2017

If you are running Visual Studio 2017, make sure you have version `15.3.26730.8` or higher.

## I'm vulnerable. Now what.

* Update Visual Studio through `Tools > Extensions and updates...`.
* [Update git](https://git-scm.com/downloads)


Stay safe my friends.
