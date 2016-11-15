---
title : "Yarn 0.17 is out. Time to ditch npm."
date: 2016-11-15 09:00
tags: [javascript,tool]
---

![Defenestrating npm. Original: http://trappedinvacancy.deviantart.com/art/Defenestration-115846260](/posts/files/yarn-0.17/out-npm.jpg)
> Original by [TrappedInVacancy](http://trappedinvacancy.deviantart.com/art/Defenestration-115846260) on DeviantArt.

If you were avoiding [Yarn][1] because of its [tendencies to delete your bower folder][2], it's time to install the latest.

Among the many changes, it [removes support for bower][3]. So yarn is truly a drop in replacement for npm now.

To upgrade:

```bash
npm install -g yarn
```

Ensure that `yarn --version` returns `0.17`. Then run it against your code base by simply typing this:

```
yarn
```

Only thing you should see is a `yarn.lock` file.

### Wait... why should I care about yarn?

First, yarn freezes your dependency when you first install them. This allows you to avoid upgrading sub-sub-sub-sub-sub-sub-sub-sub dependency that could break your build because someone down the chain didn't get [semver][4].

The lock file is alphabetically ordered YAML and automatically generated when running `yarn`. Every time this file change, you know your dependencies changed. As simple as that. Not only that, it also freezes all child dependencies as well. That makes build process repeatable and non-breaking even if someone decides that semver is stupid.

Second, yarn allows for interactive dependency upgrade. Just look at this beauty.

![Interactive Upgrade!](/posts/files/yarn-0.17/upgrade-interactive.png)

Cherry picking your upgrade has never been easier. If include `yarn why <PACKAGE NAME>` which gives you the reason for a package's existence, yarn truly allows you to see and manage your dependencies with ease.

Finally, yarn will checksum and cache every packages it downloads. Even better for build servers that always re-install the same packages. Yarn also install/download everything in parallel. Everything to get you fast and secure builds for this special Single Page Application you've been building.

If you want the whole sales pitch, you can head read so on [Facebook's announcement's page](https://code.facebook.com/posts/1840075619545360).

### What about you?

What is your favorite [Yarn][1] feature? Have you upgraded yet? Leave me a comment!

[1]: https://yarnpkg.com/
[2]: https://github.com/yarnpkg/yarn/issues/616
[3]: https://github.com/yarnpkg/yarn/pull/896
[4]: http://semver.org
