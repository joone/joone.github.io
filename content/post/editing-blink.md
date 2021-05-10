---
title: "Making Use Of Chrome's Ozone-GBM Intel Graphics Support On The Linux Desktop"
date: 2018-07-12T17:27:01-08:00
categories:
tags:
keywords:
---

Recently, I published a blog article about [making Use Of Chrome's Ozone-GBM Intel Graphics Support On The Linux Desktop](https://01.org/blogs/joone/2018/using-chrome-os-graphics-stack-intel-based-linux-desktops). This is about using ChromeOS graphics stack for a regular Linux system. This article was also mentioned in Phoronix([link](https://www.phoronix.com/scan.php?page=news_item&px=Using-Ozone-GBM-On-Desktop)).

Originally, [my old colleague started working on this project a few years ago](https://software.intel.com/en-us/blogs/2014/10/23/chromium-ozone-gbm-explained), but it has not been maintained since then. So I have been trying to enable ozone-gbm on a Linux system because this solution would be useful for embedded systems. Here is [a Yocto recipe](https://github.com/joone/meta-browser/tree/ozone-gbm-69) so you can easily create your own Linux image with Chromium ozone-gbm backend.