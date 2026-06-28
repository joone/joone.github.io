---
title: "Apple-style-span class was fully removed from Blink"
date: 2017-02-17
description: ""
tags: "Blink"
---

Finally, the Apple-style-span class has been fully removed from Blink ([commit](https://codereview.chromium.org/2685793002/)).

Apple-style-span has not been produced since 2011 (see <https://webkit.org/blog/1737/apple-style-span-is-gone>), but there was still some legacy code to handle the `Apple-style-span` class, because old WebKit engines had produced it. Now [its usage is quite low (<= 0.0001%)](https://www.chromestatus.com/metrics/feature/timeline/popularity/461), so we decided to remove it from Blink at BlinkOn7. The code ran whenever users copied text or ran editing APIs (`document.execCommand`) to keep the styles when pasting. Other non-standard CSS classes (`Apple-interchange-newline`, `Apple-converted-space`, `Apple-paste-as-quotation`) will also be removed soon if possible.

Anyway, I feel that editing in Chromium is a bit faster. :P