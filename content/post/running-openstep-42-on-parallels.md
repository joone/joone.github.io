---
title: 'Installing OpenStep 4.2 on Parallels'
date: 2017-09-20T11:38:00.002-07:00
draft: false
aliases: [ "/2017/09/running-openstep-42-on-parallels.html" ]
tags : [NextStep]
---

NextStep is one of the advanced operating systems in 90s. You may understand why it is by watching this video:  

  
Every features Steve Jobs introduced in this video was possible in 1992, which is amazing. So I've dreamed of using NextStep since I was aware of it, but I've never seen a running demo and just saw a [NexTCube](https://en.wikipedia.org/wiki/NeXTcube). By chance, I found instructions how to install OpenStep 4.2 on Parallels. Finally, NexStep(OpenStep4.2) started running on my machine. o/  

[![](https://4.bp.blogspot.com/-6d12sX2iPQ8/WcNufWlNDAI/AAAAAAAA3iE/vLcgrzOaoUE9PY2oQ-R0Q3p6JBLv2W-gQCLcBGAs/s400/Parallels%2BPicture%2B3.png)](https://4.bp.blogspot.com/-6d12sX2iPQ8/WcNufWlNDAI/AAAAAAAA3iE/vLcgrzOaoUE9PY2oQ-R0Q3p6JBLv2W-gQCLcBGAs/s1600/Parallels%2BPicture%2B3.png)

  
If you want to run OpenStep 4.2, follow [this instruction](http://openstep.bfx.re/). The only problem is that network doesn't work so it needs to install a network driver. I found [the solution](https://forum.parallels.com/threads/openstep-4-2-how-to-setup-networking.8837/) by googling, but the link of NE2000 disk image([http://www-teaching.physics.ox.ac.uk/NextStep/NE2K\_driver.fdd)](http://www-teaching.physics.ox.ac.uk/NextStep/NE2K_driver.fdd)) was broken. Fortunately, I was able to recover the link from [https://archive.org/](https://archive.org/) so you can download [it](https://www.dropbox.com/s/igsaopb8rur3r8t/NE2K_driver.fdd?dl=0).  
  
Network setting:  
1) Install the NE2K driver;  
2) Shut down. Under Boot Order, make sure the hard drive is the first device, and add "devices.net.force\_adapter\_type=rtl"; in the boot flags.  

[![](https://3.bp.blogspot.com/-N38_2lnWt0c/WcNqP8o6ScI/AAAAAAAA3h4/RDinEaRdDGkXw6N-CtImAc9i36iA8lKPgCLcBGAs/s640/Screen%2BShot%2B2017-09-21%2Bat%2B12.28.29%2BAM.png)](https://3.bp.blogspot.com/-N38_2lnWt0c/WcNqP8o6ScI/AAAAAAAA3h4/RDinEaRdDGkXw6N-CtImAc9i36iA8lKPgCLcBGAs/s1600/Screen%2BShot%2B2017-09-21%2Bat%2B12.28.29%2BAM.png)

  
3) Set the shared network. Reboot.  
5) Open HostManager.app. Under Local choose "use local domain only".  

[![](https://2.bp.blogspot.com/-O2Elf37ptRM/WcNpDnvg1cI/AAAAAAAA3hs/oGUrXdcKEu4HeaDqXmBjfLUPe0Gwu5-GQCLcBGAs/s640/Parallels%2BPicture.png)](https://2.bp.blogspot.com/-O2Elf37ptRM/WcNpDnvg1cI/AAAAAAAA3hs/oGUrXdcKEu4HeaDqXmBjfLUPe0Gwu5-GQCLcBGAs/s1600/Parallels%2BPicture.png)

  
6) Assigned one of available IPs in your local network. Let the machine Reboot;  
7) Add a name server in /etc/resolv.conf  
  
Installing developer tools: there is a good video on Youtube:  

  
The next step is to write some Objective-C code with [this book](http://www.nextcomputers.org/NeXTfiles/Docs/Software/OPENSTEP/802-2110.pdf) and download some applications from [http://www.nextcomputers.org/NeXTfiles/](http://www.nextcomputers.org/NeXTfiles/)  
  
  
References:  

*   [http://openstep.bfx.re/](http://openstep.bfx.re/)
*   [http://www.nextcomputers.org/docs/FAQ-OpenStepOnEmulators.pdf](http://www.nextcomputers.org/docs/FAQ-OpenStepOnEmulators.pdf)
*   [https://forum.parallels.com/threads/openstep-4-2-driver-thread.3408/](https://forum.parallels.com/threads/openstep-4-2-driver-thread.3408/)
*   [http://www.nextcomputers.org/forums/viewtopic.php?t=2992](http://www.nextcomputers.org/forums/viewtopic.php?t=2992)
*   [https://forum.parallels.com/threads/older-nic-emulation-before-gigabit.110324/](https://forum.parallels.com/threads/older-nic-emulation-before-gigabit.110324/)