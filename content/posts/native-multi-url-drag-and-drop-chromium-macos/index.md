---
title: "Support native multi-URL drag-and-drop in Chromium on macOS"
date: 2026-07-08
description: ""
tags: "chromium, drag-and-drop, macOS"
---

I've landed a CL for native multi-URL drag-and-drop support on macOS. We can
now drag multiple URLs using the `text/uri-list` drag data type from Chromium
and drop them into native apps such as Keynote and Pages. It also works with
Safari. This is part of the broader multi-URL drag-and-drop support effort I've
been working on in Chromium. Windows is already supported.

This change will ship in Edge/Chrome 152.

- **Change list:** [Support native multi-URL drag and drop on macOS (7774364)](https://chromium-review.googlesource.com/c/chromium/src/+/7774364)
- **Demo:** [X thread](https://t.co/cZAqvOGyBm)
