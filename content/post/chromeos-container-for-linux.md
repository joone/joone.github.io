---
title: 'Linux container for ChromeOS '
date: 2018-08-29T13:15:00.001-07:00
draft: false
aliases: [ "/2018/08/chromeos-container-for-linux.html" ]
tags : [Container, ChromeOS, Rust]
---

Recently, ChromeOS started to support Linux applications on ChomeOS. Google hasn't allowed ChromeOS to run native applications due to security reason. Finally, they found a way to support Linux application through the container technology.  
  
It is worth to read [a discussion on CrOSVM on Hacker News](https://news.ycombinator.com/item?id=15346269) because the original author joined the discussion. Here is the article about ChromeOS Linux container.  
[https://www.zdnet.com/article/chrome-os-could-be-getting-containers-for-running-linux-vms/](https://www.zdnet.com/article/chrome-os-could-be-getting-containers-for-running-linux-vms/)  
  
There is a Youtube video:  
[https://www.youtube.com/watch?v=s9mrR2tqVbQ](https://www.youtube.com/watch?v=s9mrR2tqVbQ)  
  
Here is the readme about CrOSVM.  
[https://chromium.googlesource.com/chromiumos/platform/crosvm/+/837b59f2d97b005ef84ac36efa97530c1bbf2a79/README.md](https://chromium.googlesource.com/chromiumos/platform/crosvm/+/837b59f2d97b005ef84ac36efa97530c1bbf2a79/README.md)  
  
The interesting thing is that it is implemented with Rust. Google now seriously uses Rust of their product, which is a good news for the Rust community.