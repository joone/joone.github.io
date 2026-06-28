---
title: "Camera Interface on Mobile Platform"
date: 2008-04-20
description: ""
tags: "mobile, firefox, camera"
---

The Mozilla platform is trying to support device APIs for the mobile environment. Some people call them native interfaces.

[I already posted a blog on device APIs.](http://joone4u.blogspot.com/2008/01/ideas-on-supporting-native-interfaces.html)

The Mozilla community has started to discuss which device APIs should be supported, through [the newsgroup](http://groups.google.com/group/mozilla.dev.platforms.mobile/browse_thread/thread/ff9445bb68243433) and [the Mozilla wiki](http://wiki.mozilla.org/Mobile/DeviceAPIs).

Christopher added some information about the device APIs in the wiki, so you can find the ongoing issues for supporting them. They are not confirmed yet, so you can suggest your own ideas and opinions.

Actually, I'm interested in implementing a camera interface for the Mozilla platform. This camera interface is not so important on the desktop, but it plays an important role in the mobile environment, because users can take a photo more easily using their phone.

That is why Mozilla Mobile has been interested in implementing a camera API on the Mozilla platform.

I think implementing a camera API is not that difficult, because Mozilla has already shown its video playback functionality through an HTML5 element, which uses the GStreamer framework. So we can use a camera input instead of a network stream. But you need to know how to use the camera input inside Mozilla.

Some mobile platforms provide a way to use their camera input, such as Windows Mobile 5.0 and the Maemo platform. So I'd like to introduce these APIs below.

### Maemo platform

[There is an article that introduces how to use the Camera API.](http://maemo.org/development/documentation/how-tos/4-x/how_to_use_camera_api.html) According to the article, applications can access the camera interface through a kernel API called Video4Linux. The built-in camera in the Nokia N810 looks compatible with the [Video4Linux version 2 API](http://www.thedirks.org/v4l2/), according to the article.

Fortunately, the Maemo platform delegates all multimedia handling to the GStreamer framework. This means that developers can use the Camera API if they only know how to use the GStreamer framework on the Maemo platform.

### Windows Mobile 5.0

[Windows Mobile developers can manipulate the camera input through DirectShow.](http://www.codeguru.com/cpp/g-m/bitmap/capturing/article.php/c12327/) But it requires some prior knowledge of how to use the COM interface.

Mozilla Mobile is planning to support the camera interface through JavaScript. I'm not sure whether Mozilla Mobile will expose the interface to the Web. Someday, users may be able to upload photos taken with the camera directly from Mobile Firefox to Flickr.