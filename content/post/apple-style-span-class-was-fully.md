---
title: 'Apple-style-span class was fully removed from Blink'
date: 2017-02-17T14:48:00.002-08:00
draft: false
aliases: [ "/2017/02/apple-style-span-class-was-fully.html" ]
tags : [Blink]
---

Finally, Apple-style-span class was fully removed from Blink([commit](https://codereview.chromium.org/2685793002/)).  
  
Apple-style-span has not been produced since 2011: [https://webkit.org/blog/1737/apple-style-span-is-gone](https://webkit.org/blog/1737/apple-style-span-is-gone), but there were still some legacy code to handle Apple-style-span class because old WebKit engines has produced it, but now [the usage is quite low(<=0.0001%)](https://www.chromestatus.com/metrics/feature/timeline/popularity/461). So, we decided to remove it from Blink at BlinkOn7. The code was executed whenever the users copy a text or run Editing APIs(document.execCommand) to keep the styles when pasting it. Other non-standard CSS classes(Apple-interchange-newline, Apple-converted-space, Apple-paste-as-quotation) will be also removed soon if possible.  
  
Anyway, I feel that editing in Chromium is a bit faster :P