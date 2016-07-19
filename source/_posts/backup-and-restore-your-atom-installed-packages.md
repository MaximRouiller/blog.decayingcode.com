---
title: 'Backup and restore your Atom installed packages'
date: '2016-07-19 12:00'
tags: [atom,tool,web]
---

So you are using Atom and you start installing plugins. Everything works nice and you have your environment with just the right packages.

Suddenly, your hard drive crash or maybe your whole computer burn down. Or worse, you HAVE to use a client's computer to do your work.

So you have to reconfigure your environment. How will you recover all your packages and preferences?

First, as soon as you have your packages in a good state, run the first command to backup your installed packages.

### Backing up your Atom Packages

Run this command:

```shell
apm list --installed --bare > atomPackages.txt
```

Will output something like this:

```text
angular-jonpapa-snippets@0.7.0
angularjs@0.3.4
atom-beautify@0.29.10
```

### Restoring your Atom Packages

To restore those packages, run the following command:

```shell
apm install --packages-file .\atomPackages.txt
```

Will give you an output like this:

```shell
Installing angular-jonpapa-snippets@0.7.0 to C:\Users\XXX\.atom\packages done
Installing angularjs@0.3.4 to C:\Users\XXX\.atom\packages done
Installing atom-beautify@0.29.10 to C:\Users\XXX\.atom\packages done
```

### What's `apm`?

`apm` stands for Atom Package Manager. See it as a something similar to `npm` for node but for the Atom editor instead. You can do pretty much anything you want to Atom with `apm`.

### What's left?

The only we are not backing up at this point is your snippets, custom styles, themes and your keymaps.

If you are interested, let me know and I'll show you how to back those up too.
