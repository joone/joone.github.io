---
title: "Feedbacks from the communities on the key events handling in WebKit"
date: 2010-08-04
description: ""
tags: "Webkit, W3C, IME"
---

[I posted an email to the WebKit mailing list](https://lists.webkit.org/pipermail/webkit-dev/2010-July/013684.html) about the issue I mentioned in my previous blog post. The email described the status of the inconsistent event handling during an IME composition in WebKit. Fortunately, I received quick feedback from the communities on each issue, as follows.

Issue 1) IME composition events should be handled consistently in all ports of WebKit.

=> _"This can't be achieved as it depends on the platform IME system. Therefore, different IMEs making consistent behaviors across multiple platforms is an exercise in futility. However, If we are seeing different behaviour with the same IME on a single platform, that's a bug."_

Issue 2) The `textInput` event should be dispatched after a `compositionend` event.

=> _"_[_There's a discussion on www-dom at w3.org_](http://lists.w3.org/Archives/Public/www-dom/2010AprJun/0048.html) _about changing the spec because there seems to be an issue with handling a textInput event. Currently, before the textInput event is dispatched, the DOM has already been mutated during a composition. Therefore, if there is to be a textInput event, the composition text is first removed; then, if the textEvent is not cancelled, the browser inserts the composition text again. This does not seem efficient."_

Issue 3) While a composition session is active, keyboard events should not be dispatched to the DOM.

=> Regarding keyboard events, _WebKit has fired key events during a composition for a long time. Therefore, it can't stop firing key events without breaking sites._

As a result, they agreed that [the DOM Level 3 event model](http://www.w3.org/TR/DOM-Level-3-Events/) for input composition does not match the requirements of actual web content, as I had worried. Therefore, to avoid confusion among web developers, the DOM Level 3 Events spec needs to explain why each browser handles keyboard events in different ways.