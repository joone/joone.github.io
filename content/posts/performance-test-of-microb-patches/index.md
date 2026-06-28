---
title: "Performance test of the microB patches"
date: 2008-01-23
description: ""
tags: "performance, patch, microB"
---

I have tested [the microB patches based on Mozilla 1.9 (Firefox 3 alpha6 pre)](http://browser.garage.maemo.org/docs/patches.html).

- The microB package contains more than 200 patches and several patch sets.
- It is currently released with Firefox 3 alpha6 pre.

I used two kinds of test cases:

- [Tinderbox DHTML Test](http://www.mozilla.org/performance/test-cases/dhtml/)
- [CSS Rendering](http://www.howtocreate.co.uk/csstest.html)

Target device: PAX320, RAM 256MB.

I got the following results on TestGtkEmbed in Mozilla:

- **Tinderbox DHTML Test:** the microB-patched Mozilla is 2.2% faster than Mozilla 1.9 (Firefox alpha6 pre).
- **CSS Rendering:** the microB-patched Mozilla is 5.6% slower (?) than Mozilla 1.9.

I applied the default patch set of microB to Mozilla 1.9. I can't understand why the microB patches are slower in the case of CSS Rendering.

The following items are the performance-related patches in the default patch set:

```text
# Performance improvements
perf_addon/attachment.cgi?id=246204.diff
perf_addon/new_cached_scale.gtk2.diff
perf_addon/bug54205.diff
perf_addon/540_BUG54340_js_malloc.diff
perf_addon/545_BUG54340_findkeyword_inline.diff
perf_addon/550_BUG54340_optimization_options.diff
perf_addon/thread_wait_block.diff
perf_addon/spidermonkey_alloc.diff
perf_addon/605_css_erros_parsing_disable.diff
```

There are also some patches on Cairo. I did not go through what each patch means.

Looking forward to your opinions and thoughts about the results.

Thanks.

p.s. [There is the same article on the Mozilla newsgroup.](http://groups.google.com/group/mozilla.dev.platforms.mobile/browse_thread/thread/842faccb30e3375e)
