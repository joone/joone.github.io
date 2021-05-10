---
title: 'Feedbacks from the communities on the key events handling in WebKit'
date: 2010-08-04T06:24:00.004-07:00
draft: false
aliases: [ "/2010/08/feedbacks-from-communities-on-handling.html" ]
tags : [Webkit, W3C, IME]
---

[I posted an email to the WebKit mailing list](https://lists.webkit.org/pipermail/webkit-dev/2010-July/013684.html) on the issue I mentioned in the previous blog.The email introduced a status of the inconsistent event handling during a IME composition on WebKit. Fortunately, I've got quick feedbacks from the communities on each issue as follows,  
  
Issue1) IME Composition events should be handled consistently in all ports of WebKit.  
\=> _"This can't be achieved as it depends on the platform IME system. Therefore, different IMEs making consistent behaviors across multiple platforms is an exercise in futility. However, If we are seeing different behaviour with the same IME on a single platform, that's a bug."_  
  
Issue2) The textInput event should be dispatched after a compositionend event.  
\=> _"_[_There's a discussion on www-dom at w3.org_](http://lists.w3.org/Archives/Public/www-dom/2010AprJun/0048.html) _about changing the spec because there seems a issue of handling a textInput event. Currently, before the textInput event is dispatched, DOM has been already mutated during a composition. Therefore, If there is to be a textInput event, first, the composition text is removed. And then, if the textEvent is not cancelled, the browser inserts the composition text again. It seems not efficient."_  
  
Issue3) While a composition session is active, keyboard events should not be dispatched to the DOM.  
\=> In relation to keyboard events, _WebKit has fired key events during a composition for a long time. Therefore, It can't stop to fire key events in order to avoid site break._  
  
As a result, they agreed that [the DOM Level 3 event model](http://www.w3.org/TR/DOM-Level-3-Events/) for input composition does not match the requirements of actual web content as I have worried. Therefore, in order to avoid the confusion among web developers, the DOM Level3 events spec needs to introduce the reason why each browser handles keyboard events in different ways.