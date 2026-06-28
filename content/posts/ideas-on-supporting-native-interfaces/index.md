---
title: "Ideas on supporting native interfaces in Mobile Firefox"
date: 2008-01-07
description: ""
tags: "Mobile Firefox"
---

I'd like to share my ideas on native interface support in Mobile Firefox. These ideas are based on Christian's Mobile Goals:

<http://www.christiansejersen.com/blog/2007/11/20/mobile-goals/>

## Goal

Provide a mobile web application environment that can use native interfaces.

- It can let XUL add-ons and web applications use native interfaces through JavaScript.

## Types of Native Interfaces

Most mobile platforms provide APIs that make it easy to use the camera, GPS, contacts, and so on. There are two types of native interface:

- Native interfaces from hardware:
  - GPS
  - Camera
  - Phone Call
- Native interfaces from an application engine:
  - Contact
  - SMS
  - Calendar (HTML5)

There might be more native interfaces.

## Application Examples

### GPS (Location)

- Web-based GPS navigation using Google Maps.
- Location-based web services:
  - Search
  - News
  - Auction (eBay)

Search results or lists of items could be filtered based on the user's location. It is possible for web services to get the user's location through the web browser.

### Camera

- Provide a way of handling camera input from web applications.
  - If so, Flickr could provide a web-based camera application so that users can store their photos more easily.
  - The application could use SVG transform and filter features for handling photos.
  - Users could draw on photos using the Canvas feature.

### Phone Call

- Safari on the iPhone already supports this feature via the anchor tag.
- We should support this feature in the same way.

### Contact

- Provide a way of importing a new contact from a web page via microformats, the `address` element, and the contact attribute of HTML5.

### SMS

- Provide a way of sending an SMS containing part of a web page or a URL.

### Calendar

- Provide a way of importing an event from an HTML5 element.

## Implementation

- It is possible to expose device capabilities through XPCOM, XPConnect, and JavaScript.
- Provide an abstraction layer to support cross-platform use.

## Security

- If a web service tries to use native interfaces, the browser should show a dialog asking the user to grant permission.
- Mobile Firefox should manage permitted web sites in a local database.

[Technorati Profile](http://technorati.com/claim/nqrrwnv28i)