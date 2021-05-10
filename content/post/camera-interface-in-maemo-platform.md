---
title: 'Camera Interface on Mobile Platform'
date: 2008-04-20T06:36:00.012-07:00
draft: false
aliases: [ "/2008/04/camera-interface-in-maemo-platform.html" ]
tags : [mobile, firefox, camera]
---

Mozilla platform is trying to support device APIs for mobile environment.  
Some people call them native interface  
  
[I already posted a blog on device APIs](http://joone4u.blogspot.com/2008/01/ideas-on-supporting-native-interfaces.html)  
  
The Mozilla community started to discuss what the device APIs should be supported through [the news group](http://groups.google.com/group/mozilla.dev.platforms.mobile/browse_thread/thread/ff9445bb68243433) and [Mozilla wiki](http://wiki.mozilla.org/Mobile/DeviceAPIs).  
  
Christopher added some information regarding the device APIs in the wiki.  
So you can find on-going issues to support the device APIs. They are not confirmed, so you can suggest your idea & opinion to them.  
  
Actually, I'm interested in implementing a camera interface for Mozilla platform. This camera interface is not so important on desktop, but it plays a important role in mobile environment. Because the users can take a photo more easily using their phone.  
  
That is a reason why Mozilla Mobile has been interested in implementing Camera API on Mozilla platform.  
  
I think that implementing Camera API seems to be not difficult, because Mozilla already showed its video playback functionality through HTML5 element, which uses the GStreamer framework. So we can use a camera input instead of network stream. But, you should know how to use the camera input inside Mozilla.  
  
Some of mobile platforms provide a way of using their camera input such as Windows Mobile5.0, Maemo platform. So I'd like to introduce the APIs as follows,  
  
**MaemoPlatform**  
[There is an article which introduces how to use the Camera API](http://maemo.org/development/documentation/how-tos/4-x/how_to_use_camera_api.html)  
According to the article, applications can access the Camera interface through a kernel API called Video4Linux. The built-in camera present in Nokia N810 looks compatible with [Video-4-Linux version 2 API](http://www.thedirks.org/v4l2/) from the article.  
  
Fortunately, the Maemo platform delegates all multimedia handling to the GStreamer framework. It means that developers can use the Camera API if they know only how to use the GStreamer framework on Maeme platform.  
  
**Windows Mobile 5.0**  
[Windows Mobile developers can manipulate the camera input through DirectShow](http://www.codeguru.com/cpp/g-m/bitmap/capturing/article.php/c12327/). But It needs to know some prior knowledge of how to use COM interface.  
  
Mozilla Mobile is planning to support the Camera interface through JavaScript. I'm not sure if Mozilla Mobile will expose the interface for the Web. Someday, the users would be able to upload their photos taken by the camera directly from Mobile Firefox to Flickr in any ways.