---
title: "WebKit Contributors Meeting 2011"
date: 2011-04-30
description: ""
tags: "Webkit, Conference"
---

I attended [the WebKit Contributors Meeting](http://www.webkit.org/blog/1566/its-time-for-the-2011-webkit-contributors-meeting/), held by Apple on April 24–25, 2011, near the Apple Campus in Cupertino, California. This is the annual meeting for WebKit reviewers and committers. They come together to discuss the current issues in WebKit and hack on the code during the meeting. In this post, I am going to introduce what we talked about at the WebKit meeting.

As you know, WebKit is the hottest open source project these days, because most mobile phones use the WebKit engine, and Chrome and Safari are based on WebKit as well. In addition, the WebKit engine is used on mobile platforms such as HP WebOS and the RIM PlayBook. As a result, Apple and Google, as well as other big electronics companies like Nokia, RIM, Samsung, and Motorola, have joined the WebKit project. As you might already know, [Collabora](http://collabora.com/), the company I work for, has contributed to [WebKitGtk+](http://webkitgtk.org/) along with Igalia.

[![WebKit Contributors Meeting 2011](http://farm6.static.flickr.com/5266/5685808656_31d294c4bb.jpg)](http://farm6.static.flickr.com/5266/5685808656_31d294c4bb.jpg)

On the first day, we built [the tracks and sessions](https://spreadsheets.google.com/ccc?key=0AoCAfo_LQ5_kdGs3NEJMeUNVS3JUNUx5N3NSUU1aRWc&hl=en&authkey=CI7EsvgK#gid=0) on the spot in the morning. There were [many talks and hackathons proposed](http://trac.webkit.org/wiki/April%202011%20Meeting), and we chose our favorite items by voting. Although I was not able to attend all the sessions, I will introduce some of them briefly.

First, I'd like to talk about WebKit2. In the previous year, [Apple announced the WebKit2 project](https://lists.webkit.org/pipermail/webkit-dev/2010-April/012235.html) to support a multi-process model in WebKit, like the Chrome browser. Other ports, such as Qt and Gtk+, are also trying to support the WebKit2 model. In the WebKit2 session, we discussed many issues, but I have listed some of the most important ones that readers might be interested in:

*   Using a C API: is this a good choice?
*   Support for out-of-process plugins
*   Code sharing between WebKit1 and WebKit2
*   Rewriting DRT
*   Support for a threaded model
*   The low-level communication code is very platform-specific. It needs an abstract model for communication between the web process and the UI process.
*   It is difficult to follow the accessibility implementation on the Mac.

The next interesting session was about hardware acceleration. We discussed [the current issues of hardware acceleration](http://trac.webkit.org/wiki/April%202011%20Meeting%20Hardware%20Acceleration%20Track) in each port. In particular, Google’s effort on 2D acceleration was quite interesting to me. 2D acceleration is a very challenging task, because I have seen many attempts to improve 2D graphics rendering through OpenGL or OpenVG, yet the performance was not faster than the CPU. Interestingly, Google seemed to be trying to accelerate 2D graphics with 3D graphics within WebKit itself. But not all 2D drawing depends on the GPU; only specific 2D drawing primitives are rendered by the GPU. So they are currently profiling the 2D drawing primitives rendered by the GPU to see which ones are really accelerated by it. If this hybrid model is successful, we will be able to apply it to other ports.

In other sessions, we talked about reducing the number of build systems in WebKit. Currently, we have 8 build systems, one for each WebKit port. So when we add new source files, we sometimes forget to add the files to all of the build systems, which causes build breaks. Accordingly, [CMake](http://www.cmake.org/) and [GYP](http://code.google.com/p/gyp/) were proposed for unifying the build systems. However, each port simply follows its own development environment, so it seems like it would be hard to unify the current build systems into one. I think WebKitGtk+ can move to CMake, but I’m not sure if the GNOME folks would like it. In addition, one of our core contributors, Eric, introduced how his team designed and implemented the [HTML5 parser](http://www.webkit.org/blog/1273/the-html5-parsing-algorithm/). The existing parser was so complex—written in a single source file that was the longest file in WebKit—that, as he told us, it took several months to understand the parser code. So he divided the parsing functionality into several classes, such as `HTMLToken`, `HTMLTokenizer`, `HTMLTreeBuilder`, `HTMLDocumentParser`, and others.

[![HTML5 parser session](http://farm6.static.flickr.com/5227/5655590159_d44b646aff_m.jpg)](http://farm6.static.flickr.com/5227/5655590159_d44b646aff_m.jpg)

Additionally, there was a party on the first day, hosted by the Google Chrome team. I met the reviewers who had reviewed my patches. I was pleased to meet them in person; it was like meeting old friends. It made me feel more connected to the community.

These days, the companies that the WebKit contributors work for are competing with each other, but as I saw at the meeting, the contributors work together to develop a better web engine. For instance, I learned that Google is trying to overtake Apple with Android and Chrome, yet they still work closely together to develop features of the HTML5/CSS3 standard. In addition, Nokia and RIM’s reviewers, like Kenneth and tonikitoo, always review Samsung’s patches. I think this is an example of the power of open source.

Anyway, I hope to share more details about the next meeting. Thanks!
