---
title: "Debugging Fennec front-end"
date: 2009-10-11
description: ""
tags: "fennec, Mozilla"
---

Fennec is a XUL application, like Firefox, based on the Mozilla platform. Therefore, it can be debugged and modified with the Firefox debugging tools you are already familiar with.

XUL applications consist of several XUL, JavaScript, and CSS files, which are archived in a Jar file. In the case of Fennec, it has two jar files in `fennec/chrome/`: `en-US.jar`, which holds localization information, and `chrome.jar`, which holds the Fennec front-end code.

To modify them:

1. Extract `chrome.jar` into the current path.
2. Modify `chrome.manifest` as follows:

```
override chrome://global/skin/about.css chrome://browser/skin/about.css
skin browser classic/1.0 content/..
content branding content/branding/
content firstrun content/ contentaccessible=yes
content browser content/
```

To display variable values or simple JavaScript debug messages in the system console, you can use the `dump` function. Before using it, you need to enable the browser dump preference by typing `about:config` in the URL bar:

```
browser.dom.window.dump.enabled=true
```

This is an example of using `dump()` in the `startup()` function in `browser.js`:
```
 startup: function() {  
    var self = this;  
  
    dump("begin startup\n");  
  
    let container = document.getElementById("tile-container");  
   ...
```

The `dump` function works well on Linux, but it didn't initially work when debugging Fennec for Windows Mobile in Visual Studio on Windows. After using the latest build of Fennec, it worked well in Visual Studio too, so I could see the debug messages in the Visual Studio output box.

**References**

- [http://www.getbooksmarts.org/news/2007/03/17/debug-output-logging-in-firefox/](http://www.getbooksmarts.org/news/2007/03/17/debug-output-logging-in-firefox/)
- [https://developer.mozilla.org/en/Debugging\_JavaScript](https://developer.mozilla.org/en/Debugging_JavaScript)