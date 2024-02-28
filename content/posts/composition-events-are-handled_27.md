---
title: 'IME composition events are handled inconsistently in WebKit'
date: 2010-07-26T21:33:00.017-07:00
draft: false
aliases: [ "/2010/07/composition-events-are-handled_27.html" ]
tags : [Webkit, DOM, Hangul]
---

I have been working on [Korean Hangul composition issue in WebKitGtk](https://bugs.webkit.org/show_bug.cgi?id=40518). By the way, I've noticed that IME Composition events are handled inconsistently in each WebKit port.  
  
According to [W3C DOM Level 3 events](http://dev.w3.org/2006/webapi/DOM-Level-3-Events/html/DOM3-Events.html#events-compositionevents),  
1) A browser should fire compositionstart, compositionupdate, and compositionend event during a composition.  
2) The textEvent event should be dispatched after a compositionend event if the composition has not been canceled.  
3) _While a composition session is active, keyboard events should not be dispatched to the DOM (i.e., the text composition system "swallows" the keyboard events), and only compositionupdate events may be dispatched to indicate the composition process._  
  
However, all WebKit ports handle composition & textEvent events in inconsistent ways. Even keyboard events are still dispatched to the DOM during a composition.  
  
Therefore, it is necessary to fix the problems as follows:  
1) IME Composition events should be handled consistently in all WebKit ports.  
2) Keyboard events should not be dispatched during a composition.  
3) The textInput event should be dispatched after a compositionend event.  
  

The following table shows the status of DOM events of WebKit during a [Hangul(Korean Alphabet)](http://en.wikipedia.org/wiki/Hangul) composition.

[![](http://2.bp.blogspot.com/_vr2B2ySFJkY/TE51YkaHN3I/AAAAAAAAAO4/sBtd1T3DcNc/s400/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7+2010-07-27+%EC%98%A4%ED%9B%84+2.54.30.png)](http://2.bp.blogspot.com/_vr2B2ySFJkY/TE51YkaHN3I/AAAAAAAAAO4/sBtd1T3DcNc/s1600/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7+2010-07-27+%EC%98%A4%ED%9B%84+2.54.30.png)

Korean Hangul Composition Event Test in WebKit based browsers

You can find a test case from [here.](https://bug-43020-attachments.webkit.org/attachment.cgi?id=62645)  
I filed [a bug](https://bugs.webkit.org/show_bug.cgi?id=43020) for this issue in WebKit Bugzilla.