---
title: "How to make a cab installer of Windows Mobile Fennec build"
date: 2009-04-13
description: ""
tags: "fennec"
---

When you finish building Fennec for Windows Mobile, you might ask, "How can I install Fennec on my Windows Mobile handset?" and "Where is the installer?"

The Fennec build system creates only a zip file in `objdir/mobile/dist`. You can install Fennec using this zip file, but it is cumbersome. Fortunately, the Fennec team has released Fennec for Windows Mobile as a cab installer.

So how can you make a cab installer? Move to `objdir/mobile/mobile/installer` and then run the Makefile as follows:

```sh
$ make installer
```

After that, you can find a cab installer in `objdir/mobile/dist`.