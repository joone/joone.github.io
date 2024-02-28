---
title: 'Firefox3 beta1 & Cairo Graphics'
date: 2007-12-01T06:21:00.002-08:00
draft: false
aliases: [ "/2007/12/firefox3-beta1-cairo-graphics.html" ]
tags : [graphics, Firefox3]
---

![](http://www.arcanology.com/images/firefox-box.jpg)  
[As expected, Mozilla finally has started the beta test of Firefox3 after finished the alpha8 test](http://developer.mozilla.org/devnews/index.php/2007/11/19/firefox-3-beta-1-now-available-for-download/). Actually, the official version of Firefox3 should have been released this time. However, the release date is behind the schedule as the alpha test was recently finished. Anyway, we expect that Firefox would be officially released in the first half of next year  
  
The major feature of Firefox is to use [Cairo](http://cairographics.org/) as the graphic engine for rendering all of things. Cairo is designed to provide primitives for 2-dimensional drawing across a number of different backends  
Firefox2 has only made use of Cairo in its Gecko layout engine for rendering SVG and Canvas.  
However, all graphics rendering would be done through Cairo in Firefox3. The major reason of using Cairo is that SVG, Canvas, and font rendering need to use 2D vector drawing and it allows the drawing to be accelerated by GPU. In the future, Firefox is going to have the capabilities of drawing vector graphics as much as Flash so that it allows users to paint the Web in a standard way. [We can already find out many examples of the future Web graphics when surfing the Web](http://www.croczilla.com/svg/samples/).  
Click the following links for more details.  

*   [Firefox3's new features for users  
    ](http://channy.creation.net/blog/?p=453)
*   [Firefox3's enhanced technologies for developers](http://developer.mozilla.org/ko/docs/Firefox_3_for_developers)
*   [Beta1 Release Notes](http://www.mozilla.com/en-US/firefox/3.0b1/releasenotes/)