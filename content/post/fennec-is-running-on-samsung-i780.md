---
title: 'Fennec is running on SAMSUNG i780'
date: 2009-03-27T07:33:00.005-07:00
draft: false
aliases: [ "/2009/03/fennec-is-running-on-samsung-i780.html" ]
tags : [i780, fennec, Mozilla]
---

[I was told a very good news from Blassey blog](http://blog.mozilla.com/blassey/2009/03/23/memory-dragon-slain/).  

Doug Turner solved [the test blocker](https://bugzilla.mozilla.org/show_bug.cgi?id=477956) of Fennec for Windows Mobile.  

  

I was delight to hear the new because I had tried several times to run Fennec on my SAMSUNG i780 (called Mirage in Korea). I had failed to run Fennec.  

  
[![Fennec1.0 alpha for Windows Mobile](https://farm4.static.flickr.com/3454/3389757588_bf4cc9981e_o.png)](http://www.flickr.com/photos/joone/3389757588/ "Fennec1.0 alpha for Windows Mobile by joone4u, on Flickr")  
  
[![Fennec1.0 alpha for Windows Mobile](https://farm4.static.flickr.com/3570/3388947049_2d8612c45a_o.png)](http://www.flickr.com/photos/joone/3388947049/ "Fennec1.0 alpha for Windows Mobile by joone4u, on Flickr")  
  

  

Tooday, I tried to build Fennec for Windows Mobile from the trunk of Mozilla.  

It is working well, but very slow on i780.  

  

I found several problems as follows,  

\- Multiple instances  

\- Long start-up time  

\- IME button not disappeared  

\- Broken Hangul(Korean)  
  

I will file up these problems on the Bugzilla and try to fix them.