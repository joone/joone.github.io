---
title: "libpng patch for Mobile Firefox"
date: 2008-01-06
description: ""
tags: "patch, Mozilla"
---

There are submitted patches for Mobile Firefox on the Mozilla wiki:

<http://wiki.mozilla.org/Mobile/Patches>

[My colleague tried applying the PNG patch to Mozilla and got a good result related to performance improvement.](http://groups.google.com/group/mozilla.dev.platforms.mobile/browse_thread/thread/504332475973e42d)

As you know, we can build the libpng fixed-point routines as in the patch above. (Use `#define PNG_NO_FLOATING_POINT_SUPPORTED` in `mozilla/modules/libimg/png/mozpngconf.h`.)

In this case, he got an 8.6% performance improvement. That's great. I think this patch is valuable for Mobile Firefox.

However, there is the following problem with this patch. Mozilla handles some floating-point values to get image information from libpng, so the patch comments out those parts in the `info_callback()` function (`mozilla/modules/libpr0n/decoders/png/nsPNGDecoder.cpp`). The problem is that the color management feature needs to get color profile information from libpng. The following code shows the example:

```cpp
void
info_callback(png_structp png_ptr, png_infop info_ptr)
{
...

if (gfxPlatform::IsCMSEnabled()) {
  decoder->mInProfile = PNGGetColorProfile(png_ptr, info_ptr,
                                           color_type, &inType, &intent);
}
...
}
```

We can check the value of [`gfx.color_management.enabled`](http://kb.mozillazine.org/Gfx.color_management.enabled) through the `gfxPlatform::IsCMSEnabled()` method.

If `gfx.color_management.enabled` is true, you can use the color profiles embedded in images to adjust the colors to match your computer's display. In this case, Mozilla should call the `PNGGetColorProfile()` method. But this method handles floating-point values.

Fortunately, Mozilla sets `gfx.color_management.enabled` to false by default, so the `PNGGetColorProfile()` method is not called. Nevertheless, the patch comments out this part because a user may try to set the value to true. Anyway, the color management feature is basically not used now.

If Mobile Firefox should use the color management feature, the PNG patch needs more tweaks.

What do you think about that?