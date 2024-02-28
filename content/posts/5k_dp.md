---
title: "5k tiled dual DP (two-pipe, two-port) display sync issues"
images:
  - "https://gitlab.freedesktop.org/drm/intel/uploads/b41cac264bdaa37cd259a367410ae001/5l9a7426.jpg"
date: 2022-10-15T22:49:25+00:00
author: "Joone Hur"
tags: ["x-window"]
categories: ["desktop"]
draft: false

---

https://gitlab.freedesktop.org/drm/intel/-/issues/27

5K 모니터를 우분투 20.04에서 설정하다가 재미있는 xrandr 사용 방법을 알게되었다. 
아직까지 리눅스 데스크탑 여전히 일반 사용자가 쓰기 어려운 이유는 하드웨어가 제대로 지원되지 않는 부분이다. 물론 많이 좋아졌지만,  Nvida GPU를 제대로 설정하기란 어려운 일이다.
어쩔 수 없이 open source version을 설치하고서나야 겨우 외장 모니터를 사용할 수 있었는데, 5k출력이 잘 안된다.
5k 출력은 두  DP 포트를 이용해서 화면에 절반씩 렌더링하는 구조로 되어 있는 듯 하다. 아마 하나의  DP가  bandwidth가 못받쳐줘서 그런 듯 보인다.

위 글을 보면 몇년간 사람들이 문제를 해결하기 위해 고군분투한 내용을 볼 수 있다.
```
xrandr --output DP-1 --off --output DP-2 --off
xrandr --newmode 2560x2880_MagicMode 483.25 2560 2608 2640 2720 2880 2883 2893 2962 +hsync -vsync
xrandr --addmode DP-1 2560x2880_MagicMode
xrandr --addmode DP-2 2560x2880_MagicMode
xrandr --output DP-1 --mode 2560x2880_MagicMode --output DP-2 --mode 2560x2880_MagicMode
xrandr --output DP-1 --pos 0x0 --output DP-2 --pos 2560x0 --output eDP-1 --auto --pos 5120x720
```
xrandr은  kernel mode setting을 이용해서  display를 다시  configure할 수 있다. 위와 같이 하면 5k모니터에서 풀 해상도를 즐길 수 있다.
하지만 내가 가진  hp laptop에서는 잘 안된다. 아마  ubuntu 22.04를 사용하면 되지 않을까 싶다.
