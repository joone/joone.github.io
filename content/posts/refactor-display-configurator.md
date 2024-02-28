---
title: "Single interface for Display Configuration"
date: 2019-02-27T11:06:10-08:00
draft: true
categories:
tags:
keywords: 
---

When I fist looked into DisplayManager and DisplayConfigurator of ChromeOS code,
I was able to understand that they are need to configure displays attached to a Chromebook: getting display modes from an internal display or external display via the display controller. Display modes specify a combination of parameters, not only the display resolution but also refresh rate, colour depth and signal timings(vsync).

However, it's hard to figure out how DisplayManager and DisplayConfigurator work together.
There are also a lot of other clasess that DisplayConfigurator uses such as DislayLayoutManager, 
두 클래스가 어떻게 일을 나누어 처리하하는지 서로간의 관계를 

In my opinion, 