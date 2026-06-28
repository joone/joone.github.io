---
title: "Enable to open multiple file-pickers in Chromium for Linux"
date: 2017-03-01
description: ""
tags: "Blink"
---

Sometimes we don't know the exact requirements or anticipate user behavior while developing or fixing something. [I fixed the file-picker modal issue in Chromium for Linux last year](https://codereview.chromium.org/1624793002/). At that time, the reviewer and I thought there would be no case where multiple file-pickers pop up, but [it happened](https://bugs.chromium.org/p/chromium/issues/detail?id=678982) since M55 when you follow these steps:

1. Enable "Ask where to save" in settings.
2. Open 2 tabs of, e.g., https://sourceforge.net/projects/azureus/files/latest/download
3. Wait for 2 downloader windows, then close/cancel both.
4. Freeze or crash.

Here is a video to reproduce the problem.

When a file-picker is opened, it disables event listening on the main host window. When the user closes the file-picker, event listening is enabled again. Now the host window has a counter to track the number of open file-pickers, and it does not disable event listening as long as any file-picker is open. Event listening is only re-enabled when the last file-picker is closed. Here is [the fix](https://codereview.chromium.org/2709283003/).

Anyway, you may see the fix in M58 (Apr 25th, 2017).