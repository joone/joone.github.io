---
title: 'Building Chromium browser on your Mac'
date: 2010-08-07T01:03:00.013-07:00
draft: false
aliases: [ "/2010/08/building-chromium-browser-on-your-mac.html" ]
tags : [OSX, chromium]
---

[![](http://4.bp.blogspot.com/_vr2B2ySFJkY/TF0anmRCrOI/AAAAAAAAAQI/BqU1d_Zqg74/s320/200px-Chromium_Logo.png)](http://4.bp.blogspot.com/_vr2B2ySFJkY/TF0anmRCrOI/AAAAAAAAAQI/BqU1d_Zqg74/s1600/200px-Chromium_Logo.png)

Copyright © Chromium Project

Building your own browser is the best way to find bugs and fix them. Now, I'd like to introduce how to build Chromium on your mac. I think that the latest code seems more stable(?) and provides more features.  
  
This is a starting point to build Chromium on Mac OS X.  
[http://code.google.com/p/chromium/wiki/MacBuildInstructions](http://code.google.com/p/chromium/wiki/MacBuildInstructions)  

#### Installing the depot-tools

http://dev.chromium.org/developers/how-tos/install-gclient  
  
$ svn co http://src.chromium.org/svn/trunk/tools/depot\_tools  
  
Add depot\_tools to your PATH:  
$ export PATH=\`pwd\`/depot\_tools:"$PATH"  

#### Getting the code

A checkout straight from the Subversion (SVN) repository can take a long time. It would be good to download a tar ball from [http://build.chromium.org/buildbot/archives/chromium\_tarball.html](http://build.chromium.org/buildbot/archives/chromium_tarball.html)  
  
Then, you can update the code to the latest revision from the SVN repository.  
$ gclient sync  

#### Building from the command line

To build all targets (it takes a long time due to test case builds)  
$ cd ~/chromium/src/build  
$ xcodebuild -project all.xcodeproj -configuration Debug -target All  
  
To build just chrome:  
$ cd ~/chromium/src/chrome  
$ xcodebuild -project chrome.xcodeproj -configuration Debug -target chrome  

#### Running Chromium

Run Chromium debug build  
$ cd ~/chromium/src/xcodebuild/Debug  

$ open ./Chromium.app  
  

Run Chromium release build

$ cd ~/chromium/src/xcodebuild/Release

$ open ./Chromium.app

  
The latest revision of Chromium looks cool.  

[![](http://4.bp.blogspot.com/_vr2B2ySFJkY/TF0X1-J59mI/AAAAAAAAAQA/_0XSnj6iIOM/s400/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7+2010-08-07+%EC%98%A4%ED%9B%84+5.15.36.png)](http://4.bp.blogspot.com/_vr2B2ySFJkY/TF0X1-J59mI/AAAAAAAAAQA/_0XSnj6iIOM/s1600/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7+2010-08-07+%EC%98%A4%ED%9B%84+5.15.36.png)