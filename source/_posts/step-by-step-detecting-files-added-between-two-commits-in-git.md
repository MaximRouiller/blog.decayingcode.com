---
title: "Step by step: Detecting files added between two commits in git"
date: 2018-07-19 10:23:00
tags: [git]
---

I was looking into retrieving the last created files into a repository. For me, it was for our [Microsoft Azure Documentation](https://docs.microsoft.com/azure?WT.mc_id=maximerouiller-blog-marouill). 

This is, of course, completely open source and you can find [the repository on GitHub](https://github.com/MicrosoftDocs/azure-docs). The problem however is that a ton of persons work on this and you want to know what are the new pages of docs that are created. It matters to me because it allow me to see what people are creating and what I should take a look into.

So, how do I retrieve the latest file automatically?

Knowing that `git show HEAD` shows you the latest commit on the current branch and `git show HEAD~1` shows you the previous commit on the current branch, all we have to do is make a `diff` out of those two commits.

# Showing changes between two commits

```bash
git diff HEAD HEAD~1
```

This however will show you in great details all the files that have been modified including their contents. Let's trim it down a bit to only show names and status.

# Showing names and status of files changed between two commits

```bash
git diff --name-status HEAD HEAD~1
```

Awesome! But now, ou should see the first column filled with a letter. Sometimes `A`, sometimes `D` but most often `M`. `M` is for modified, `D` for deleted and `A` for added. The last one is the one that I want.

Let's add a filter on that too.

# Showing names of files added between two commits

```bash
git diff --name-only --diff-filter=A HEAD HEAD~1
```

At that point, I changed `--name-status` to `--name-only` since now we are guaranteed to only have added files in our list and I don't need the status column anymore. The thing however, is that I'm seeing `png` files as well as other types of files that I'm not interested in. How do I limit this to only markdown files?

# Showing names of markdown files added between two commits

```bash
git diff --name-only --diff-filter=A HEAD HEAD~1 *.md
```

And that's it. That's how a simple command coupled with a few parameters can allow you total control of what you want out of git.

# Resources

Here are the resources I used to build this command:

* [git-diff documentation](https://git-scm.com/docs/git-diff)