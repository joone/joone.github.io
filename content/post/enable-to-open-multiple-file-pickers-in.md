---
title: 'Enable to open multiple file-pickers in Chromium for Linux'
date: 2017-03-01T13:06:00.001-08:00
draft: false
aliases: [ "/2017/03/enable-to-open-multiple-file-pickers-in.html" ]
tags : [Blink]
---

Sometimes, we don't know exact requirements or expect user behaviors while developing or fix something. [I fixed the file-picker modal issue in Chromium for Linux last year](https://codereview.chromium.org/1624793002/). At that time, the reviewer and I thought that there would be no case to pop-up multiple file-pickers, but [it happened](https://bugs.chromium.org/p/chromium/issues/detail?id=678982)  since M55 when you follow the following steps:  
1) Enable "Ask where to save" in settings.  
2) Open 2 tabs of e.g.: https://sourceforge.net/projects/azureus/files/latest/download  
3) Wait for 2 downloader windows, and close/cancel/etc both  
4) Freeze or crash  
  
Here is a video to reproduce the problem.  
  
  
  
When file-picker is opened, it disables event listening of the main host window. Then, the user closes the file-picker, it enables the event listening. Now, the host widow has a counter to check the number of the open file-pickers and it doesn't disable the event listening if there are any open file-picker. The event listening can be enabled when the last file-picker is closed. Here is [the fix](https://codereview.chromium.org/2709283003/).  
Anyway, you may see the fix in M58(Apr 25th, 2017)