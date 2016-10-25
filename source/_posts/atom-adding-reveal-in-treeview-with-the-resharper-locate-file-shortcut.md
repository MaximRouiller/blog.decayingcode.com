---
title : "Atom: Adding Reveal in TreeView with the ReSharper Locate File Shortcut"
date: 2016-10-25 09:00
tags: [atom]
---

There's one feature I just love about ReSharper is the [`Locate File in Solution Explorer`](https://www.jetbrains.com/help/resharper/2016.2/Navigation_and_Search__Locating_a_File_in_Solution_Explorer.html) in Visual Studio.

For me, Visual Studio is too heavy for JavaScript development. So I use Atom and I seriously missed this feature.

Until I added it myself. With 2 lines of ~~code~~ declaration.

```coffee
'atom-text-editor':
    'alt-shift-l': 'tree-view:reveal-active-file'
```

What other shortcuts are you guys using?
