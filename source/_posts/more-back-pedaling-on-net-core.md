---
title: "More back pedaling on .NET Core"
date: 2016-06-15 08:00:00
tags: [asp.net,opinion]
---

>They say great science is built on the shoulders of giants. Not here. At Aperture, we do all our science from scratch. No hand holding. - Cave Johnson

Removing `project.json` was explained by the sheer size of refactoring every other projects to unify in one model. It was a justified explanation.

After reading that the .NET team is [removing grunt/gulp][1] from the project templates, I'm left to wonder what is the motivation behind it. Apparently, [some users had issues with it][3] and it had to be pulled. No more details were given.

In fact, they are [pulling back the old bundler/minifier][2] into the fray to keep the bundling/minifying feature.

All this so we can do `dotnet bundle` without requiring other tools. Everything will be unified once more. No need for external tooling or `node`. Everything will be Microsoft tools.

Now to explain to my client why we are using `node` to automate our workflow since Microsoft definitely won't be including it in its templates. It really makes you think twice about offering directions to clients.

[1]: https://github.com/aspnet/Tooling/issues/83#issuecomment-225967707
[2]: https://github.com/aspnet/Templates/pull/592
[3]: https://github.com/OmniSharp/generator-aspnet/issues/733#issuecomment-224386200
