---
title: 'libpng patch for Mobile Firefox'
date: 2008-01-06T04:42:00.001-08:00
draft: false
aliases: [ "/2008/01/libpng-patch-for-mozilla-firefox.html" ]
tags : [patch, Mozilla]
---

There are submitted patches for Mobile Firefox on the Mozilla wiki.  
[http://wiki.mozilla.org/Mobile/Patches](http://wiki.mozilla.org/Mobile/Patches)  
  
[My colleague tried to apply the png patch to Mozilla and get a good result related to performance improvement.](http://groups.google.com/group/mozilla.dev.platforms.mobile/browse_thread/thread/504332475973e42d)  
  
As you know, we can build the libpng fixed point routines like the above patch.  
(Use #define PNG\_NO\_FLOATING\_POINT\_SUPPORTED in mozilla/modules/libimg/png/mozpngconf.h )  
  
In this case, he can get 8.6% performance improvement.  
It's great. I think this patch is valuable to use on Mobile Firefox.  
  
However, there is the following problem in this patch.  
Mozilla handles some floating point values to get image information from the libpng so the patch commented those parts in the info\_callback() function. (mozilla/modules/libpr0n/decoders/png/nsPNGDecoder.cpp)  
The problem is that color management feature should get color profile information from the libpng. The following code shows the example:  
  
void  
info\_callback(png\_structp png\_ptr, png\_infop info\_ptr)  
{  
...  
  
if (gfxPlatform::IsCMSEnabled()) {  
decoder->mInProfile = PNGGetColorProfile(png\_ptr, info\_ptr,  
color\_type, &inType, &intent);  
}  
...  
}  
  
We can check the value of [gfx.color\_management.enabled](http://kb.mozillazine.org/Gfx.color_management.enabled) through the gfxPlatform::IsCMSEnable() method.  
  
If the gfx.color\_management.enabled is true, you can use the color profiles embedded in images to adjust the colors to match your computer's display. In this case, Mozilla should call the PNGGetColorProfile() method. But this method handles floating point values.  
  
Fortunately, Mozilla sets the value of gfx.color\_management.enabled to false as default. The PNGGetColorProfile() method is not called. Nevertheless, the patch commented this part because a user may try to set the value to true.  
Anyway, the color management feature is not used basically now.  
  
  
If Mobile Firefox should use the color management feature, the png patch needs more tweaks.  
  
How do you think about that?