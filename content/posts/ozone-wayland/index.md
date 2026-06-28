---
title: "Ozone-Wayland"
date: 2014-04-02
description: ""
tags: "ozone-wayland, chromium, wayland"
---

Ozone-Wayland is an Ozone implementation for Chromium that allows [Crosswalk](http://crosswalk-project.org/) and the Chromium browser to run natively on Wayland without any X11 dependency [1].

I have been working on Ozone-Wayland recently. There have been two releases since I got involved in the development. [In the latest release](https://github.com/01org/ozone-wayland/tree/Milestone-Easter), I contributed virtual keyboard support to Ozone-Wayland. You can see how it works in the following video:

The Ozone-Wayland team has been focusing on graphics accelerations such as WebGL, Canvas 2D, and accelerated compositing on Wayland. WebGL and Canvas 2D can be accelerated by [off-screen rendering](https://github.com/01org/ozone-wayland/issues/29) in the GPU process. In the latest release, we started supporting multi-touch and the virtual keyboard, which work fine on Tizen IVI, as you can see in the video above.

## What is Ozone?

[Ozone](http://www.chromium.org/developers/design-documents/ozone) is an abstraction layer used by Chromium browsers to separate out the different windowing systems and to abstract surface acceleration for the Aura UI framework, input handling, event handling, and other UI-related matters [4]. Ozone-Wayland provides Wayland support for Ozone [2].

## References

1. Project homepage: <https://github.com/01org/ozone-wayland>
2. <https://01.org/ozone-wayland/blogs/kalyankondapally/2014/beta-channel-updated-m35>
3. [Ozone-Wayland Release Adds Virtual Keyboard, Touch Support](http://www.phoronix.com/scan.php?page=news_item&px=MTY0Mzc), Mar. 26, 2014
4. [Chromium On Wayland "Ozone" Continues](http://www.phoronix.com/scan.php?page=news_item&px=MTQ3OTE), Oct. 07, 2013
5. [Wayland-Based Chromium Browser Released](http://www.phoronix.com/scan.php?page=news_item&px=MTUxMTA), Nov. 11, 2013
6. [Chromium Ported To Wayland, Now Working](http://www.phoronix.com/scan.php?page=news_item&px=MTQ2NDY), Sep. 18, 2013
