---
title: 'The new Headless mode now supports disabling lazy loading'
date: 2023-05-07T16:06:00.003-07:00
categories: ["chromium"]
draft: false
aliases: [ "/2023/05/headless-disable-lazy-loading.html" ]
tags : [chromium, headless]
---

Recently, [support for disabling lazy loading was added to the new headless Chromium](https://chromium-review.googlesource.com/c/chromium/src/+/4510361). 
Since the Google Chrome team announced t[he new headless mode](https://developer.chrome.com/articles/new-headless/), the new headless mode has gradually supported the old headless features. This was one of the missing features. 

I note that "--disable-lazy-loading" is not a runtime flag for the headless modes because it is now added by default.
But, What does --disable-lazy-loading actually mean?

First, we need to understand what lazy image loading is on the Web.
Lazy loading is a technique that delays the loading of resources until they are needed. This can improve performance by reducing the amount of data that needs to be loaded initially. 

For example, when  a  web page has numerous images, the user may have to scroll down the page to view the images. In such cases, the browser can defer the loading of the unseen images by using loading=lazy attributes for images. See this demo: [https://enviragallery.com/demo/lazy-loading-demo/](https://enviragallery.com/demo/lazy-loading-demo/)
So, why should we disable lazy loading in headless mode?
In headless mode,  the image lazy loading becomes unnecessary because users want to get a fully loaded page in order to capture a screenshot or generate a pdf.  Without injecting JS code, it is impossible to scroll the page in headless mode.
This applies not only to image loading but also to any sub-resources like JavaScript, CSS, and iframes as follows:


```
<img src="image.jpg" alt="..." loading="lazy" />
<iframe src="video-player.html" title="..." loading="lazy"></iframe>
```
This change was applied to the main branch of headess_shell and old headless mode on Jan 18, 2023

You can find more details about it here:

[https://source.chromium.org/chromium/chromium/src/+/a4e84e7e99130c902d10209bb33903c110e359e7](https://source.chromium.org/chromium/chromium/src/+/a4e84e7e99130c902d10209bb33903c110e359e7)

If you wish to disable lazy loading, make sure you are using the latest version of Chromium.

Here are some additional details about lazy loading:
* [https://developer.mozilla.org/en-US/docs/Web/Performance/Lazy_loading](https://developer.mozilla.org/en-US/docs/Web/Performance/Lazy_loading)
* [https://web.dev/i18n/en/browser-level-image-lazy-loading/](https://web.dev/i18n/en/browser-level-image-lazy-loading/)
