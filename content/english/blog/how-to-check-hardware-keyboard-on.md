---
title: 'Fixing a hardware keyboard problem in Mobile Firefox for Windows Mobile '
date: 2009-08-27T07:36:00.003-07:00
draft: false
aliases: [ "/2009/08/how-to-check-hardware-keyboard-on.html" ]
tags : [fennec, Windows Mobile]
---

Finally, m[y patch was applied to the mainline of Mozilla](http://hg.mozilla.org/releases/mozilla-1.9.2/rev/56eb284b6862) and [this cumbersome bug has been fixed](http://dougt.org/wordpress/2009/03/windows-ce-hardware-keyboard-help/). I am so happy to contribute to Mozilla. Here is the detail what the problem was fixed:  
  
Windows Mobile keeps the status of hardware keyboard in the registry as follows:  
  
```
"HKEY\_CURRENT\_USER\\SOFTWARE\\Microsoft\\Shell\\HasKeyboard"
```  
A software keyboard can pop-up if a device doesn't have a hardware keyboard. However, in the case that the device has a slide-out keyboard, the value should be changed according to the status of the slide-out keyboard.  
  
For example:  
  
1) When the keyboard does not slide out, the HasKeyboard value has to be "0"  
2) Whenthe keyboard slides out, the HasKeyboard value has to be "1"  
  
However, the above registry value, HasKeyboard, always returns 1 because it doesn't consider the status of slide-out keyboard. :-(  
  
Windows Mobile doesn't have a standard API for detecting a status of slide-out keyboard so each device maker has to provide its own hidden or public API. For Samsung Windows Mobile devices, Samsung provides [Windows Mobile SDK](http://innovator.samsungmobile.com/down/cnts/toolSDK.list.do?platformId=2) from the Samsung Mobile Innovator site so we can easily get a state of the slide-out keyboard on Samsung devices. However, HTC doesn't provide any document on how to use device APIs.  
  
[As a result, some hackers had tried to find out how to use hidden APIs for accessing hardware features of HTC devices](http://msmobiles.com/news.php/7477.html), but there is no API about slide-out keyboard.  
  
Anyway, [Fennec has a bug with detecting the state of slide-out keyboard](https://bugzilla.mozilla.org/show_bug.cgi?id=341772), which seemed easy to solve. At least, it was easy for my i780, but my first patch couldn't support HTC touch pro that has a side-out keyboard.  
  
I tried to check most of the registry values if there is any change. Finally, I found the value from  HKEY\_LOCAL\_MACHINE\\System\\GDI\\Rotation\\Slidekey. That made me fix the bug easily. :-)