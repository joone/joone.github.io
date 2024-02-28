---
title: 'WebKit Contributors Meeting 2011'
date: 2011-04-30T21:04:00.002-07:00
draft: false
aliases: [ "/2011/05/webkit-contributors-meeting-2011.html" ]
tags : [Webkit, Conference]
---

I attended [the WebKit Contributors meeting](http://www.webkit.org/blog/1566/its-time-for-the-2011-webkit-contributors-meeting/) which was held by Apple, April 24~25 2011 near Apple Campus in Cupertino, California. This is the annual meeting for WebKit reviewers and committers. They come together to discuss the current issues of WebKit and hack the code in the meeting. In this post, I am going to introduce you to what we talk about in the WebKit meeting.  
  
As you know, WebKit is the hottest open source project these days, because most mobile phones use the WebKit engine and additionally Chrome and Safari also are based on WebKit. In addition, the WebKit engine is used for mobile platforms such as HP WebOS and RIM Playbook. Therefore, Apple and Google as well as other big electronics companies like Nokia, RIM, Samung, Motorola have joined the WebKit project. As you might already know, [Collabora](http://collabora.com/), the company I work for, has contributed to [WebKitGtk+](http://webkitgtk.org/) with Igalia.  
  

[![](http://farm6.static.flickr.com/5266/5685808656_31d294c4bb.jpg)](http://farm6.static.flickr.com/5266/5685808656_31d294c4bb.jpg)

  
On the first day, we made [the tracks and sessions](https://spreadsheets.google.com/ccc?key=0AoCAfo_LQ5_kdGs3NEJMeUNVS3JUNUx5N3NSUU1aRWc&hl=en&authkey=CI7EsvgK#gid=0) instantly in the morning. Actually, there were [many talks and hackathons proposed](http://trac.webkit.org/wiki/April%202011%20Meeting), but we chose our favorite items by voting. Although I was not able to attend all sessions, I will introduce some of them briefly.  
  
First, I'd like to talk about WebKit2. Actually, [Apple announced the WebKit2 project](https://lists.webkit.org/pipermail/webkit-dev/2010-April/012235.html) to support a multiple process model in WebKit like Chrome browser in the previous year. Other ports like Qt and Gtk+ are also trying to support the WebKit2 model. In the WebKit2 session, we discussed many issues, but I listed up some of the most important ones which readers might be interested in as follows:  
  
\- Using C API, is this a great choice?  
\- Support for Out-of-process plugin  
\- Code sharing between WebKit1 and WebKit2  
\- DRT rewritten  
\- Support for threaded model  
\- Low-level communication code is very specific to the platform. It needs an abstract model for communication between the web process and ui process.  
\- It is difficult to follow accessibility implementation on a Mac.  
  
The next interesting session was about hardware acceleration. We discussed [the current issues of hardware acceleration](http://trac.webkit.org/wiki/April%202011%20Meeting%20Hardware%20Acceleration%20Track) in each port. In particular, Google’s effort on 2D acceleration is quite interesting to me. Actually, 2D acceleration is a very challenging task because I have seen many trials of improving 2D graphics rendering through OpenGL or OpenVG.  However, the performance was not faster than CPU. Interestingly, Google seemed to try to accelerate 2D graphics by 3D graphics in the WebKit itself. But, all 2D drawings are not dependent on GPU, only the specific 2D drawing primitives are rendered by GPU. So they are currently profiling 2D drawing primitives rendered by GPU to see which 2D graphics primitives are really accelerated by GPU. If this hybrid model is successful, we will be able to apply it to other ports.  
  
In other sessions, we talked about reducing the build systems in WebKit. Currently, we have 8 build systems for each WebKit port. So, when we add new source files, sometimes we forget to add the files to all of the build systems, which causes build breaks. Accordingly, [CMake](http://www.cmake.org/) and [GYP](http://code.google.com/p/gyp/) were proposed for unifying the build systems. However, each port just follows its development environment, so it seems like it would be hard to unify each current build system into one. I think that WebKitGtk+ can move to CMake, but I’m not sure if the GNOME guys like it. In addition, one of our core contributors, Eric introduced how his team designed and implemented [HTML5 parser](http://www.webkit.org/blog/1273/the-html5-parsing-algorithm/).  Actually, the existing parser was so complex as written in one source file which was the longest file in WebKit, so he told us that it took several months to understand the parser code. So he divided the parsing functionality into several classes such as HTMLToken, HTMLTokenizer, HTMLTreeBuilder, HTMLDocumentParser, and others.  

[![](http://farm6.static.flickr.com/5227/5655590159_d44b646aff_m.jpg)](http://farm6.static.flickr.com/5227/5655590159_d44b646aff_m.jpg)

  
Additionally, there was a party on the first day held by the Google Chrome Team. I met the reviewers who have reviewed my patches. I was pleased to meet them directly; it was like meeting old friends. It made me feel more connected to the community.  
  
These days, the companies in which the WebKit contributors are involved are competing with each other, but as I saw at the meeting, the contributors are working together to develop a better web engine. For instance, I learned that Google is trying to overtake Apple with Android and Chrome, but they still work closely to develop the features of the HTML5/CSS3 standard. In addition, Nokia and RIM’s reviewers, like Kenneth and tonikitoo always review Samsung’s patches. I think this is an example of the power of open source.  
  
Anyway, I hope to talk more details about the next meeting. Thanks!