---
title: 'How to make a cab installer of Windows Mobile Fennec build'
date: 2009-04-13T06:41:00.003-07:00
draft: false
aliases: [ "/2009/04/how-to-make-cab-installer-of-windows.html" ]
tags : [fennec]
---

When you finish building Fennec for Windows Mobile, you can ask "How can I install Fennec on my Windows Mobile handset?"  
  
"Where is the installer?"  
  
Fennec build system creates only a zip file in the path of "objdir/mobile/dist".  
  
You might install Fennec using this zip file. But, it is cumbersome. Fortunately, the Fennnec team has released Fennec for Windows Mobile as a cab install.  
  
"How can they make a cab installer?"  
  
If you want to make a cab installer, move to /objdir/mobile/mobile/installer.  
And then, run Makefile as follows,  
  
$make installer  
  
If so, you can find a cab installer in the path of "objdir/mobile/dist".