---
title: "Firefox3 beta1 & Cairo Graphics"
date: 2007-12-01
description: ""
tags: "graphics, Firefox3"
---

![Firefox box artwork](http://www.arcanology.com/images/firefox-box.jpg)

[As expected, Mozilla has finally started the beta test of Firefox3 after finishing the alpha8 test](http://developer.mozilla.org/devnews/index.php/2007/11/19/firefox-3-beta-1-now-available-for-download/). The official version of Firefox3 should actually have been released by now. However, the release date has slipped behind schedule, as the alpha test only recently finished. Anyway, we expect that Firefox3 will be officially released in the first half of next year.

The major feature of Firefox3 is using [Cairo](http://cairographics.org/) as the graphics engine for rendering everything. Cairo is designed to provide primitives for two-dimensional drawing across a number of different backends.

Firefox2 used Cairo only in its Gecko layout engine for rendering SVG and Canvas. In Firefox3, however, all graphics rendering is done through Cairo. The main reason for using Cairo is that SVG, Canvas, and font rendering all need 2D vector drawing, and it allows the drawing to be accelerated by the GPU. In the future, Firefox will be able to draw vector graphics as richly as Flash, letting users paint the Web in a standard way. [We can already find many examples of the future of Web graphics while surfing the Web](http://www.croczilla.com/svg/samples/).

Click the following links for more details:

* [Firefox3's new features for users](http://channy.creation.net/blog/?p=453)
* [Firefox3's enhanced technologies for developers](http://developer.mozilla.org/ko/docs/Firefox_3_for_developers)
* [Beta1 Release Notes](http://www.mozilla.com/en-US/firefox/3.0b1/releasenotes/)
