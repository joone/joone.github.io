---
title: "Using Nokia N9 in Korea"
date: 2012-03-26
description: ""
tags: "Nokia, collabora_planet, N9, MeeGo"
---

[It’s been almost 3 months since I started using a Nokia N9](http://robswain.tumblr.com/post/15234704977/nokia-n9) as my secondary phone, which is [a Linux-based smartphone released by Nokia in October 2011](http://www.engadget.com/2011/10/22/nokia-n9-review/). In this post, I will introduce what the Nokia N9 is and how to use it in Korea, because it adopts an open source mobile platform, which means that anyone can get involved with the project.

## Nokia N9

Unfortunately, the N9 is the first and last MeeGo phone, but it seems to be close to the Maemo platform, because it uses Qt as the UI framework and the Debian package system. In addition, [the N9 is the first mobile phone to embrace WebKit2, which supports the multi-process model](http://ariya.ofilabs.com/2011/08/first-look-at-nokia-n950-web-browser.html), so [you can build WebKit for the Nokia N9](http://trac.webkit.org/wiki/SettingUpDevelopmentEnvironmentForN9).

## Korean Support

![Nokia N9 with a Korean keyboard](https://lh4.googleusercontent.com/HPj23pI87uzGfFaK9rG8Ky_JER75f1qZX6xQpIcL5Whr5IpTykSpiWSWOMO8YMP_Hf7BUkBKzdkprG--GZu1w9JxyPkz_QsfPRP7l3nlwVHSKBrqVA8)

Actually, I had been having trouble using the N9 because it does not officially support the Korean language, so there is no Korean keyboard or Korean fonts by default. Fortunately, [a Nokia Korean engineer](http://talk.maemo.org/showthread.php?t=78578) developed [a Hangul keyboard and a Debian package with Korean fonts](http://shootspeak.com/2011/10/13/korean-input-support-for-nokia-n9-alpha-release/). I think he might have worked on this in his free time. I am very grateful for his efforts. You can find [the source code of the Korean keyboard on Gitorious](https://gitorious.org/nokia-n9-hangul/meegohangul).

## N9 User Group in Korea

In addition, there is [a small N9 user group in Korea](http://cafe.naver.com/ArticleList.nhn?search.clubid=18321033&search.menuid=256&search.boardtype=L), so I got some information on how to register the N9 with Korean wireless telecommunication operators such as SK Telecom and KT. Interestingly, the saleswoman in the phone shop was able to set it up without asking any questions while registering it, which shows that the N9 UI is very intuitive to use.

## Developer Support

![Nokia N9 developer tools](https://lh6.googleusercontent.com/jHmDwm_bXSDDiq2YzwOlCAXrN7KRxCH19YOe4j02w1i58ca_2xeWbODBIi8X9xlpNVE7LhXHK2YlDavX8BgNmjlmb45hUDVFjepSX2nOB-ixFaXgfy4)

[It's quite easy to install developer packages on the N9.](http://harmattan-dev.nokia.com/docs/library/html/guide/html/Developer_Library_Developing_for_Harmattan_Activating_developer_mode.html) You can run an X terminal and browse the directories to check which system libraries are installed. Furthermore, if you want to develop a Qt application, [you can even install the Qt SDK and developer tools on Linux, Windows, and Mac](http://www.developer.nokia.com/Devices/MeeGo/).

## Installing Gtk+

Gtk+ had been used as the default UI widget until Maemo Fremantle, but the N9 started using Qt by default. Fortunately, a hacker ported [Gtk+ for Maemo Fremantle](http://repository.maemo.org/pool/maemo5.0/free/g/gtk+2.0/) on N9, so [you can install the same Gtk+ 2.14.7](http://talk.maemo.org/showthread.php?t=79229) on N9.

## What about the next model of the N9?

I think Nokia did not give up on their generic Linux platform, even though they chose the Windows Phone platform instead of MeeGo. I expect that Nokia will continue releasing Qt-based smartphones in the future, because a true Linux platform will be their hidden key someday.

Anyway, if you want to make your own mobile platform or test its component software, the N9 would be a good test bed for you.