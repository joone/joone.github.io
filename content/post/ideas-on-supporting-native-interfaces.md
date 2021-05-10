---
title: 'Ideas on supporting native interfaces in Mobile Firefox'
date: 2008-01-07T08:00:00.000-08:00
draft: false
aliases: [ "/2008/01/ideas-on-supporting-native-interfaces.html" ]
tags : [Mobile Firefox]
---

I'd like to share my ideas on native interface support in Mobile Firefox.  
These ideas are based on Christian's Mobile Goal.  
http://www.christiansejersen.com/blog/2007/11/20/mobile-goals/  
  
Goal  
Provide mobile web application environment which can use native interfaces.  
\- It can make XUL add-ons & web applications use native interfaces  
through JavaScript  
  
Types of Native Interfaces  
(Most of mobile platforms support APIs for easy of use for camera, GPS, contact, and etc..)  
  
There are two types of native interface:  
\* native interfaces from hardware  
\- GPS  
\- Camera  
\- Phone Call  
  
\* native interfaces from application engine  
\- Contact  
\- SMS  
\- Calendar (HTML5)  
  
There might be more native interface.  
  
Application Examples  
  
GPS (Location)  
  
\* Web based GPS Navigation using Google Map  
  
\* Location based Web Service  
\- Search  
\- News  
\- Auction(e-bay)  
  
Search results or list of items could be found related to user's location.  
It is possible that web services gets user's location through web browser.  
  
  
Camera  
  
\* Provide a way of handling camera input from web applications.  
o If so, Flickr provides a web based camera application and so user can store their photos more easily.  
o The applcation can use SVG transform & filter features for handling photos.  
o User can draw on photos using Canvas feature.  
  
  
Phone Call  
  
\* Safari on the iPhone already supports this feature via anchor tag.  
  
\* We should support this feature like that.  
  
  
Contact  
  
\* Provide a way of importing a new contact from a web page via micro format, address element, and contact attribute of HTML5  
  
  
SMS  
  
\* Provide a way of sending a SMS with containing of a part of web page or URL.  
  
  
Calendar  
  
\* Provide a way of importing event from element of HTML5  
  
  
Implementation  
  
\* It is possible to expose device capabilities through XPCOM, XPConnect, and JavaScript.  
  
\* Provide a abstract layer for supporting cross platform  
  
  
Security  
  
\* If web services try to use native interfaces, the browser should show a dialog to users for grant.  
  
\* Mobile Firefox should manage permitted web sites in local database.  
  
[Technorati Profile](http://technorati.com/claim/nqrrwnv28i)