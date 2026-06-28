---
title: "Fennec is running on SAMSUNG i780"
date: 2009-03-27
description: ""
tags: "i780, fennec, Mozilla"
---

[I read some very good news on Blassey's blog](http://blog.mozilla.com/blassey/2009/03/23/memory-dragon-slain/).

Doug Turner solved [the test blocker](https://bugzilla.mozilla.org/show_bug.cgi?id=477956) of Fennec for Windows Mobile.

I was delighted to hear the news, because I had tried several times to run Fennec on my SAMSUNG i780 (called Mirage in Korea) but had always failed.

[![Fennec1.0 alpha for Windows Mobile](https://farm4.static.flickr.com/3454/3389757588_bf4cc9981e_o.png)](http://www.flickr.com/photos/joone/3389757588/ "Fennec1.0 alpha for Windows Mobile by joone4u, on Flickr")

[![Fennec1.0 alpha for Windows Mobile](https://farm4.static.flickr.com/3570/3388947049_2d8612c45a_o.png)](http://www.flickr.com/photos/joone/3388947049/ "Fennec1.0 alpha for Windows Mobile by joone4u, on Flickr")

Today, I tried to build Fennec for Windows Mobile from the Mozilla trunk. It works well, but it is very slow on the i780.

I found several problems, as follows:

- Multiple instances
- Long start-up time
- IME button not disappearing
- Broken Hangul (Korean)

I will file these problems on Bugzilla and try to fix them.