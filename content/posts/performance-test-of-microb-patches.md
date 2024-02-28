---
title: 'Performance test of the microB patches'
date: 2008-01-23T04:45:00.001-08:00
draft: false
aliases: [ "/2008/01/performance-test-of-microb-patches.html" ]
tags : [performance, patch, microB]
---

I have tested [the microB patches based on Mozilla1.9(Firefox3 alpha6 pre.)  
](http://browser.garage.maemo.org/docs/patches.html)  
\- The microB package contains more 200 patches and several patch sets.  
\- It is released with Firefox3 alpha6 pre. now.  

Two kinds of test case are used.  
\- [Tinderbox DHTML Test](http://www.mozilla.org/performance/test-cases/dhtml/)  
\- [CSS Rendering](http://www.howtocreate.co.uk/csstest.html)  

Target Device : PAX320, RAM 256MB  

And I got the following results on TestGtkEmbed in Mozilla.  

\* Tinderbox DHTML Test:  
The microB patched Mozilla is 2.2 % faster than Mozilla1.9 (Firefox alpah6 pre).  

\* CSS Rendering:  
The microB patched Mozlla is 5.6 % slower (?) than Mozilla1.9

I applied the default patch set of microB to Mozilla1.9  

I can't understand why the microB patches is slow in the case of CSS Rendering.  

The following items are performance related patches in the default patch set.  

#Performance improvements  
perf\_addon/attachment.cgi?id=246204.diff  
perf\_addon/new\_cached\_scale.gtk2.diff  
perf\_addon/bug54205.diff  
perf\_addon/540\_BUG54340\_js\_malloc.diff  
perf\_addon/545\_BUG54340\_findkeyword\_inline.diff  
perf\_addon/550\_BUG54340\_optimization\_options.diff  
perf\_addon/thread\_wait\_block.diff  
perf\_addon/spidermonkey\_alloc.diff  
perf\_addon/605\_css\_erros\_parsing\_disable.diff

There are also some patches on Cairo.  

I did not go through what each patch means.  

Looking forward to your opinion & thought about the results.  

Thanks

  

p.s. [There is the same article on Mozilla newsgroup.](http://groups.google.com/group/mozilla.dev.platforms.mobile/browse_thread/thread/842faccb30e3375e)