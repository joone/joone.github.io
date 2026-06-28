---
title: "Fixing a hardware keyboard problem in Mobile Firefox for Windows Mobile"
date: 2009-08-27
description: ""
tags: "fennec, Windows Mobile"
---

Finally, [my patch was applied to the mainline of Mozilla](http://hg.mozilla.org/releases/mozilla-1.9.2/rev/56eb284b6862) and [this cumbersome bug has been fixed](http://dougt.org/wordpress/2009/03/windows-ce-hardware-keyboard-help/). I am so happy to contribute to Mozilla. Here are the details of what the problem was:

Windows Mobile keeps the status of the hardware keyboard in the registry, as follows:

```
HKEY_CURRENT_USER\SOFTWARE\Microsoft\Shell\HasKeyboard
```

A software keyboard can pop up if a device doesn't have a hardware keyboard. However, if the device has a slide-out keyboard, the value should change according to the state of that keyboard.

For example:

1. When the keyboard is not slid out, the `HasKeyboard` value should be `0`.
2. When the keyboard is slid out, the `HasKeyboard` value should be `1`.

However, the `HasKeyboard` registry value always returns `1`, because it doesn't take the state of the slide-out keyboard into account. :-(

Windows Mobile doesn't have a standard API for detecting the state of a slide-out keyboard, so each device maker has to provide its own hidden or public API. For Samsung Windows Mobile devices, Samsung provides a [Windows Mobile SDK](http://innovator.samsungmobile.com/down/cnts/toolSDK.list.do?platformId=2) from the Samsung Mobile Innovator site, so we can easily get the state of the slide-out keyboard on Samsung devices. HTC, however, doesn't provide any documentation on how to use its device APIs.

[As a result, some hackers tried to figure out how to use the hidden APIs for accessing the hardware features of HTC devices](http://msmobiles.com/news.php/7477.html), but there was no API for the slide-out keyboard.

Anyway, [Fennec had a bug detecting the state of the slide-out keyboard](https://bugzilla.mozilla.org/show_bug.cgi?id=341772), which seemed easy to solve. At least it was easy for my i780, but my first patch couldn't support the HTC Touch Pro, which also has a slide-out keyboard.

I tried checking most of the registry values for any change. Finally, I found the value at `HKEY_LOCAL_MACHINE\System\GDI\Rotation\Slidekey`. That let me fix the bug easily. :-)
