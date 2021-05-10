---
title: 'Debugging Fennec front-end'
date: 2009-10-11T08:17:00.006-07:00
draft: false
aliases: [ "/2009/10/debugging-fennec-front-end.html" ]
tags : [fennec, Mozilla]
---

Fennec is a XUL application, like Firefox based on Mozilla Platform. Therefore, it can be debugged and modified with the Firefox debugging tools you are familiar with.  
  
XUL applications consist of several XUL, JavaScript, and CSS files, which are archived in a Jar file. In the case of Fennec, it has two jar files in fennec/chrome/. en-US.jar which has localization information, chrome.jar which has Fennec front-end code.  
  
To modify them,  
  
1) Extract the chrome.jar in the current path  
2) Modify the chrome.manifest as follows  

> override chrome://global/skin/about.css chrome://browser/skin/about.css  
> skin browser classic/1.0 content/..  
> content branding content/branding/  
> content firstrun content/ contentaccessible=yes  
> content browser content/

In order to display variable values or simple JavaScript debug message information in the system console, you can use the dump function. Before using this command, it needs to enable browser dump preferences by typing about:config in the URL bar.  

> browser.dom.window.dump.enabled=true

This is an example of using dump() in the startup() funciton in browser.js  
```
 startup: function() {  
    var self = this;  
  
    dump("begin startup\n");  
  
    let container = document.getElementById("tile-container");  
   ...
```  
The dump function works well on Linux system, but it doesn't work in Visual Studio in Windows system in order to debug Fennec for Windows Mobile.  
It works well in Visual Studio, so I could see debug messages in the output box of Visual Studio using the latest build of Fennec.  
  
**References**  

*   [http://www.getbooksmarts.org/news/2007/03/17/debug-output-logging-in-firefox/](http://www.getbooksmarts.org/news/2007/03/17/debug-output-logging-in-firefox/%20)  
    
*   [https://developer.mozilla.org/en/Debugging\_JavaScript](https://developer.mozilla.org/en/Debugging_JavaScript)