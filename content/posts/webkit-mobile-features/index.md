---
title: "WebKit Mobile Features"
date: 2011-05-21
description: ""
tags: "Webkit, mobile"
---

Recently, I spent some time implementing the viewport meta tags and the tiled backing store for WebKitGtk+, which are mobile features of the WebKit engine. In this post, I’m going to tell you more about the mobile features of the WebKit engine and how to enable each one when you build WebKitGtk+.

First of all, the WebKit engine is particularly strong in the mobile area, because it is lighter and faster than other browser engines and has been proven by the success of Safari on the iPhone. For this reason, most mobile devices have adopted the WebKit engine to offer their own web browser: Nokia Symbian, Google Android, HP WebOS, and Samsung Bada all use WebKit-based browsers.

Additionally, WebKit is an open source project. Naturally, [all source code is open to the public](http://trac.webkit.org/browser), but there is no obligation for manufacturers to open their browser code, because the WebKit engine uses the LGPL, which allows a browser executable to simply link with the WebKit engine dynamically. This means the browser can be a separate file, independent from the WebKit library. For this reason, the mobile features of Safari on the iPhone do not need to be open to the public, and consequently it has been hard to access those mobile features. Fortunately, other phone manufacturers have participated in the WebKit project and are developing mobile features within the community. Currently, Nokia, RIM, Samsung, Motorola, and Ericsson are participating in the WebKit project. In addition, the open source companies [Collabora](http://collabora.co.uk/) and Igalia have worked on WebKit for a long time.

Recently, some mobile features have already been applied to the WebKit engine, such as the tiled backing store, touch events, and viewport meta tags. Since these features are a little hard to understand from their names alone, I will explain them, along with the other mobile features supported by WebKit, in more detail.

*   Fast Mobile Scrolling
*   Tiled Backing Store
*   Viewport Meta Tags
*   Frameset Flattening
*   Touch Events

## Fast Mobile Scrolling

[![Fixed background image while scrolling](http://farm6.static.flickr.com/5182/5597019959_50c470de37.jpg)](http://farm6.static.flickr.com/5182/5597019959_50c470de37.jpg)

You can see a fixed background image (the smile icon) even while you scroll the web page, as shown in the picture above. This is possible when a web page has elements with the CSS `background-attachment` property. In this case, WebKit performs a slow repaint in order to avoid rendering artifacts. However, scrolling a web page with a fixed background image causes noticeable delays on mobile devices. In other words, WebKit tries not to update fixed elements on every scroll, because it is hard to display a fixed element while scrolling quickly.

[![Scrollable fixed background with fast mobile scrolling](http://farm6.static.flickr.com/5305/5597587400_3768649994.jpg)](http://farm6.static.flickr.com/5305/5597587400_3768649994.jpg)

To avoid this problem, we can make scrolling faster by ignoring the CSS property `background-attachment: fixed`, enabling the fast mobile scrolling option as you can see in the left picture (i.e. the fixed background becomes scrollable on WebKitGtk+ with the fast mobile scrolling build option).

To enable fast mobile scrolling, build WebKitGtk+ with the fast mobile scrolling option:

```
$WebKit/Tools/Scripts/build-webkit --gtk --fast-mobile-scrolling
```

## Tiled Backing Store

When I first used Safari on the iPhone, I was impressed by how fast scrolling web pages was. This is possible because Safari caches the content of the web page as bitmaps in memory, and the cached bitmaps are simply painted on the screen, without sending paint calls to WebCore, when we scroll the page. These bitmaps are created and deleted on demand as the viewport moves over the web page. We call them the “tiled backing store.”

[The tiled backing store was applied to QtWebKit by Nokia engineers](https://bugs.webkit.org/show_bug.cgi?id=35146). They applied this feature to WebCore so that it can be shared by other ports. In the case of WebKitGtk, [Code Aurora engineers submitted an initial patch, and I updated the patch to use only the Cairo API by removing the GTK+ dependency.](https://bugs.webkit.org/show_bug.cgi?id=45423) This allows the WinCairo and EFL ports to use the updated patch to enable the tiled backing store.

To enable the tiled backing store:

1.  Build WebKitGtk+ with the tiled backing store option as follows:

    ```
    $ WebKit/Tools/Scripts/build-webkit --gtk --tiled-backing-store
    ```

2.  Set the tiled backing store setting to `TRUE` through `WebKitWebSettings` as follows:

    ```c
    WebKitWebSettings *settings = webkit_web_view_get_settings(webView);
    g_object_set(G_OBJECT(settings), "enable-tiled-backing-store", TRUE, NULL);
    ```

## Viewport Meta Tags

[![Web page scaled to fit the screen](http://farm6.static.flickr.com/5292/5460755418_d6d667e6e1_o.png)](http://farm6.static.flickr.com/5292/5460755418_d6d667e6e1_o.png) [![Mobile web page on the iPhone](http://farm6.static.flickr.com/5252/5481285644_9a0d39c365_o.png)](http://farm6.static.flickr.com/5252/5481285644_9a0d39c365_o.png)

Apple introduced full browsing for the first time when they released the iPhone. Until then, mobile phones were not usable for browsing web pages built for the desktop, due to the small screen size. To overcome this limitation, Safari on the iPhone automatically zooms web pages out to fit the screen, as in the picture above (left).

To do this, Safari on the iPhone assumes that web pages are 980px wide by default. When Safari loads a web page, it scales the page down by a factor of 0.32 (320/980) to fit the width of the screen in the case of the iPhone 3G. But the problem is that not all pages are designed to fit a 980px width. For example, mobile web pages can display well at 320px or 480px. The Google Search page is displayed on the iPhone like the right picture, because it is designed to be narrower than 980px.

To solve this problem, Apple defined the viewport meta tags to let web browsers support both desktop and mobile web pages effectively. This is not a web standard, but most mobile web browsers have adopted it, and WebKit supports it too.

Let’s take a look at the viewport meta tag in more detail. The viewport meta tags can be used to set the width, height, and initial scale factor of the viewport. Although most web pages don’t have viewport meta tags, mobile browsers with a 320px screen width can apply initial values to web pages as follows:

```html
<meta name="viewport" content="width=980" initial-scale="0.32">
```

In the case of mobile pages, you need to set the viewport meta tags as follows:

```html
<meta name="viewport" content="user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, width=device-width" />
```

As you can see from these properties, web pages cannot be scaled up or down; even user scaling is turned off. This is because the mobile web pages are actually written for the small screen.

For more information on the viewport meta tags, refer to [the Safari reference library](http://developer.apple.com/safari/library/documentation/appleapplications/reference/safarihtmlref/articles/metatags.html).

## Frameset Flattening

[![A web page with a scrollable sub frame](http://farm6.static.flickr.com/5263/5597820692_2085d92442.jpg)](http://farm6.static.flickr.com/5263/5597820692_2085d92442.jpg)

Some web pages have their own scrollable area with scroll bars, like the page above. We call this a “sub frame.” On touch devices, the problem is that it is undesirable to have any scrollable sub frame in the web page, because the user can get confused—sometimes scrolling the sub frame and at other times scrolling the web page itself—since it is very hard for the user to understand what a “sub frame” is. For this reason, iframes and framesets are hardly usable on touch devices.

[![A web page with frameset flattening enabled](http://farm6.static.flickr.com/5262/5597236979_36bdab6dc0.jpg)](http://farm6.static.flickr.com/5262/5597236979_36bdab6dc0.jpg)

However, if you enable the frameset flattening feature when building WebKit, all the frames become one scrollable page, as you can see in the figure above.

To enable frameset flattening:

```c
WebKitWebSettings *settings = webkit_web_view_get_settings(webView);
g_object_set(G_OBJECT(settings), "enable-frame-flattening", TRUE, NULL);
```

## Touch Events

Supporting multi-touch has become popular since the iPhone introduced it. Now, all smartphones support multi-touch. Multi-touch lets users use more than two fingers to interact with applications on their smartphone. For example, you can play piano or guitar applications using all five fingers.

However, multi-touch is not only possible for native applications, but also for web pages. Since WebKit supports touch events as DOM events, web developers can handle touch events using JavaScript. There are 4 kinds of touch events: `touchstart`, `touchmove`, `touchend`, and `touchcancel`. Unfortunately, WebKitGtk+ doesn't support touch events yet. So if you want to [test the touch events](https://bug-32434-attachments.webkit.org/attachment.cgi?id=44955), running QtWebKit or mobile Safari would be a good option. For more information on how to use touch events, refer to [the Safari reference library](http://developer.apple.com/library/safari/#documentation/UserExperience/Reference/TouchEventClassReference/TouchEvent/TouchEvent.html#//apple_ref/doc/uid/TP40009358).

Finally, I’d like to mention that there are more mobile features I didn’t cover here, such as intelligent zoom, kinetic scrolling, and zooming animation. WebKit doesn’t support them by itself, but the hosting application can implement these kinds of features, because they tend to depend on the platform. However, when these features are added to WebKitGtk+, I will talk about them in more detail.

References

*   [QtWebKit goes Mobile](http://codeposts.blogspot.com/2010/06/qtwebkit-goes-mobile.html)
*   [https://trac.webkit.org/wiki/Mobile%20Features%20Talk](https://trac.webkit.org/wiki/Mobile%20Features%20Talk)
