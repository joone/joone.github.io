---
title: 'Ozone-Wayland'
date: 2014-04-02T13:24:00.001-07:00
categories: ["chromeos"]
draft: false
aliases: [ "/2014/04/ozone-wayland.html" ]
tags : [ozone-wayland, chromium, wayland]
---

Ozone-Wayland is an Ozone implementation of Chromium, which allows to run [Crosswalk](http://crosswalk-project.org/) and Chromium browser natively on Wayland without any X11 dependence\[1\].  
  
I have been working on Ozone-Wayland recently. There were two releases since I was involved in the development. [In the latest release](https://github.com/01org/ozone-wayland/tree/Milestone-Easter), I contributed the virtual keyboard support to Ozone-Wayland. You can find how it works in the following video:  

  
The ozone-wayland team has been focusing on graphics accelerations such as WebGL, Canvas 2D, Accelerated Compositing on Wayland. WebGL and Canvas 2D accelerations can be accelerated by [off-screen Rendering](https://github.com/01org/ozone-wayland/issues/29) in GPU process. In the latest release, we started supporting multi-touch and virtual keyboard, which work fine on Tizen IVI as you can the above video.  
  
What is Ozone?  
 [Ozone](http://www.chromium.org/developers/design-documents/ozone) is an abstraction layer used by Chromium browsers to separate out the different windowing systems and also abstract surface acceleration for Aura UI framework, input handling, event handling, and other UI-related matters\[4\]. Ozone-Wayland provides Wayland support for Ozone\[2\].  
  
Reference  
  

1.  Project Homepage: [https://github.com/01org/ozone-wayland](https://github.com/01org/ozone-wayland)
2.  [https://01.org/ozone-wayland/blogs/kalyankondapally/2014/beta-channel-updated-m35](https://01.org/ozone-wayland/blogs/kalyankondapally/2014/beta-channel-updated-m35)
3.  [Ozone-Wayland Release Adds Virtual Keyboard, Touch Support](http://www.phoronix.com/scan.php?page=news_item&px=MTY0Mzc), Mar. 26,  2014 
4.  [Chromium On Wayland "Ozone" Continues](http://www.phoronix.com/scan.php?page=news_item&px=MTQ3OTE), Oct. 07, 2013
5.  [Wayland-Based Chromium Browser Released](http://www.phoronix.com/scan.php?page=news_item&px=MTUxMTA), Nov. 11, 2013
6.  [Chromium Ported To Wayland, Now Working](http://www.phoronix.com/scan.php?page=news_item&px=MTQ2NDY), Sep. 18, 2013