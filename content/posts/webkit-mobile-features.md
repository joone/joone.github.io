---
title: 'WebKit Mobile Features'
date: 2011-05-21T01:04:00.000-07:00
draft: false
aliases: [ "/2011/07/webkit-mobile-features.html" ]
tags : [Webkit, mobile]
---

Recently, I spent some time implementing the viewport meta tags and tiled backing store for WebKitGtk+, which are the mobile features of the WebKit engine. In this post, I’m going to tell you more about the mobile features of the WebKit engine and how to enable each feature when you build WebKitGtk+.  
  
First of all, the WebKit engine is particularly strong in the mobile area, because it is lighter and faster than other browser engines and has been verified by the success of Safari on the iPhone. Due to this reason, most mobile devices have adopted the WebKit engine to offer their own web browser: Nokia Symbian, Google Android, HP WebOS, and Samsung Bada are all using WebKit based browsers.  
  
Additionally, WebKit is an open source project. Naturally, [all source code is open to the public](http://trac.webkit.org/browser), but there is no obligation for manufacturers to open their browser code, because the WebKit engine uses the LGPL, so it allows a browser executable file to just link with the WebKit engine dynamically. It means that the browser can be a different file independent from the WebKit library. Because of this reason, the mobile features of Safari on the iPhone need not be open to the public and subsequently it has been hard to access the mobile features. Fortunately, other phone manufacturers have participated in the WebKit project and are developing mobile features within the community. Currently, Nokia, RIM, Samsung, Motorola and Erricson have participated in the WebKit project. In addition, the open source companies [Collabora](http://collabora.co.uk/) and Igalia have worked on WebKit for a long time.  
  
Recently, some mobile features already have been applied to the WebKit engine such as the tiled backing store, touch events, and viewport meta tags. Since these features are a little complicated to understand in terms of the name, I will explain them including the other mobile features supported by WebKit in more details.  

*   Fast Mobile Scrolling
*   Tiled Backing Store
*   Viewport Meta Tags
*   Frameset Flattening
*   Touch Events

  
Fast Mobile Scrolling  
  

[![](http://farm6.static.flickr.com/5182/5597019959_50c470de37.jpg)](http://farm6.static.flickr.com/5182/5597019959_50c470de37.jpg) 

  
You can see a fixed background image(Smile Icon) when you even scroll the web page as you can see in the above picture. This can be possible if a web page has elements which have the CSS background-attachment property. In this case, WebKit carries out a slow repaint in order to avoid rendering artifacts. However, scrolling a web page with a fixed background image causes noticeable delays on mobile devices. It means that WebKit tries not to update fixed elements every time to scroll fast because it is hard to display a fixed element while scrolling fast.  
  

[![](http://farm6.static.flickr.com/5305/5597587400_3768649994.jpg)](http://farm6.static.flickr.com/5305/5597587400_3768649994.jpg)

  
In order to avoid this problem, we can make scrolling faster if we ignore the CSS property "background-attachment: fixed" by enabling the fast mobile scrolling option as you can see in the left picture (i.e. the fixed background can be scrollable on WebKitGtk+ with the fast mobile scrolling build option).  
  
To enable the fast mobile scrolling:  
  
1) Build WebKitGtk+ with the fast mobile scrolling option:  
```
$WebKit/Tools/Scripts/build-webkit --gtk --fast-mobile-scrolling
```  
  
Tiled Backing Store  
  
When I tried to use Safari on the iPhone, I was impressed by the fact that the scrolling web pages were very fast. This is possible because Safari caches the content of the web page as bitmaps in memory, and the cached bitmaps we've seen are just painted on the screen without sending paint calls to WebCore when we scroll a web page. These bitmaps are created and deleted on-demand as the viewport moves over the web page. We call them the “tiled backing store.”  
  
[The tiled backing store was applied to QtWebKit  by Nokia engineers](https://bugs.webkit.org/show_bug.cgi?id=35146). They applied this feature to WebCore, so it can be shared by other ports. In the case of WebKitGtk, [Code Aurora engineers submitted an initial patch, I have updated the patch to use only Cairo API by removing GTK+ dependency.](https://bugs.webkit.org/show_bug.cgi?id=45423) This would allow the WinCairo and Efl port to use the updated patch for enabling the tiled backing store.  
  
To enable the tiled backing store:  
1 ) Build WebKitGtk+ with the tiled backing store option as follows:  
```
$ WebKit/Tools/Scripts/build-webkit --gtk --tiled-backing-store
```  
2) Set the tiled backing store setting to TRUE through WebKitWebSettings as follows:  
```
WebKitWebSettings \*settings = webkit\_web\_view\_get\_settings(webView);  
g\_object\_set(G\_OBJECT(settings), "enable-tiled-backing-store", TRUE, NULL);   

```  
Viewport Meta Tags  

  

[![](http://farm6.static.flickr.com/5292/5460755418_d6d667e6e1_o.png)](http://farm6.static.flickr.com/5292/5460755418_d6d667e6e1_o.png) [![](http://farm6.static.flickr.com/5252/5481285644_9a0d39c365_o.png)](http://farm6.static.flickr.com/5252/5481285644_9a0d39c365_o.png) 

  
Apple introduced the full browsing feature for the first time when they released the iPhone. Until then, the mobile phone was not usable to browse web pages made for desktop due to the small screen size. To overcome this limitation, Safari on the iPhone zooms web pages out automatically to fit to the screen like in the above picture(left).  
  
For this, Safari on the iPhone assumes that web pages have 980px width by default. When Safari loads a web page, it scales down the web page by a factor of 0.32 (320/980) to fit to the width of  the screen in the case of iPhone 3G. But, the problem is that all pages are not displayed to fit to 980px width. For example, mobile web pages can be displayed well with 320px or 480px. the Google Search page is displayed on the iPhone like the right picture because the Google search page is required to have the less width compared to 980px.  
  
To solve this problem, Apple defined the viewport meta tags to allow web browsers to support desktop web page and mobile web page effectively. Actually, this is not a web standard, but most mobile web browsers have adopted this feature and WebKit also supports it.  
  
Let’s take a look at the viewport meta tag in more detail. The viewport meta tags can be used to set the width, height and initial scale factor of the viewport. Although most web pages don’t have viewport meta tags, mobile browsers which have 320 px screen width can apply initial values to web pages as follows:  
  
```
<meta name = "viewport" content = "width = 980" initial-scale = "0.32">
```  
In the case of mobile pages, you need to set the viewport meta tags as follows:  
```
<meta name="viewport" content="user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, width=device-width" />
```  
As you can see each property, web pages can not be scaled up/down; even user scaling is turned off. This is because the mobile web pages are actually written for the small screen.  
  
For more information on the viewport meta tags, refer to [the Safari reference library](http://developer.apple.com/safari/library/documentation/appleapplications/reference/safarihtmlref/articles/metatags.html).  
  
Frameset Flattening  
  

[![](http://farm6.static.flickr.com/5263/5597820692_2085d92442.jpg)](http://farm6.static.flickr.com/5263/5597820692_2085d92442.jpg)

  
There are some web pages that have their own scrollable area with the scroll bars like the above page. We call this "sub frame." On touch devices, the problem is that it is desirable not to have any scrollable sub frame of the web page because the user could confuse with sometimes scrolling sub frames and scrolling the web page itself at other times because it is very hard for the user to understand what “sub frame” is. For this reason, iframes and framesets are hardly usable on touch devices.  

[![](http://farm6.static.flickr.com/5262/5597236979_36bdab6dc0.jpg)](http://farm6.static.flickr.com/5262/5597236979_36bdab6dc0.jpg)

  
However, if you enable the frameset flattening features when building Webkit, all the frames can be one scrollable page as you can see the above figure.  
  
To enable the Fameset flattening:  
```
WebKitWebSettings \*settings = webkit\_web\_view\_get\_settings(webView);  
g\_object\_set(G\_OBJECT(settings), "enable-frame-flattening", TRUE, NULL);  

```  
Touch events  
  
Supporting multi-touch has become popular since iPhone introduced it. Now, all smart phones support multi-touch. Using multi-touch allows the users to use more than two fingers to interact with applications on their smart phone. For example, you can use a piano or guitar applications using your 5 fingers.  
  
However, using multi-touch is not only possible for native applications, but also for web pages. Since WebKit supports touch events as DOM events, web developers can deal with touch events using JavaScript. There are 4 kinds of touch events: touchstart, touchmove, touchend, and touchcancel. Unfortunately, WebKitGtk+ doesn't support the touch events yet. So if you want to [test the touch events](https://bug-32434-attachments.webkit.org/attachment.cgi?id=44955) , running QtWebKit or mobile Safari would be good. For more information on how to use the touch events, refer to [the Safari reference library](http://developer.apple.com/library/safari/#documentation/UserExperience/Reference/TouchEventClassReference/TouchEvent/TouchEvent.html#//apple_ref/doc/uid/TP40009358).  
  
Finally, I’d like to let you know there are more mobile features I didn’t introduce here, such as intelligent zoom, kinetic scrolling, zooming animation, and etc. WebKit doesn’t support them by itself, but the hosting applications can implement these kind of features, because these features are likely to depend on the platform. However, when these features are added to WebKitGtk+, I will talk about them in more detail.  
  
References  

*   [QtWebKit goes Mobile](http://codeposts.blogspot.com/2010/06/qtwebkit-goes-mobile.html)
*   [https://trac.webkit.org/wiki/Mobile%20Features%20Talk](https://trac.webkit.org/wiki/Mobile%20Features%20Talk)