---
title: "Linux Container for ChromeOS"
date: 2018-07-12
description: ""
tags: "ChromeOS, chromium, Linux"
---

Recently, ChromeOS started to support Linux applications. Google hadn't allowed ChromeOS to run native applications due to security reasons. Finally, they found a way to support Linux applications through container technology.

It is worth reading the discussion about CrOSVM on Hacker News because the original author joined the discussion. Here is an article about the ChromeOS Linux container:

<https://www.zdnet.com/article/chrome-os-could-be-getting-containers-for-running-linux-vms/>

There is also a YouTube video:

<https://www.youtube.com/watch?v=s9mrR2tqVbQ>

Here is [the README](https://chromium.googlesource.com/chromiumos/platform/crosvm/+/837b59f2d97b005ef84ac36efa97530c1bbf2a79/README.md) about CrOSVM.

The interesting thing is that it is implemented in Rust. Google now seriously uses Rust for its products, which is good news for the Rust community.

