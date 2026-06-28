---
title: "Installing OpenStep 4.2 on Parallels"
date: 2017-09-20
description: ""
tags: "NextStep"
---

NeXTSTEP was one of the advanced operating systems of the '90s. You can understand why by watching this video:

Every feature Steve Jobs introduced in this video was possible in 1992, which is amazing. I had dreamed of using NeXTSTEP ever since I became aware of it, but I had never seen a running demo and had only seen a [NeXTcube](https://en.wikipedia.org/wiki/NeXTcube). By chance, I found instructions on how to install OpenStep 4.2 on Parallels, and NeXTSTEP (OpenStep 4.2) finally started running on my machine. o/

[![OpenStep 4.2 running on Parallels](https://4.bp.blogspot.com/-6d12sX2iPQ8/WcNufWlNDAI/AAAAAAAA3iE/vLcgrzOaoUE9PY2oQ-R0Q3p6JBLv2W-gQCLcBGAs/s400/Parallels%2BPicture%2B3.png)](https://4.bp.blogspot.com/-6d12sX2iPQ8/WcNufWlNDAI/AAAAAAAA3iE/vLcgrzOaoUE9PY2oQ-R0Q3p6JBLv2W-gQCLcBGAs/s1600/Parallels%2BPicture%2B3.png)

If you want to run OpenStep 4.2, follow [this instruction](http://openstep.bfx.re/). The only problem is that the network doesn't work, so you need to install a network driver. I found [the solution](https://forum.parallels.com/threads/openstep-4-2-how-to-setup-networking.8837/) by googling, but the link to the NE2000 disk image (`http://www-teaching.physics.ox.ac.uk/NextStep/NE2K_driver.fdd`) was broken. Fortunately, I was able to recover the link from [the Internet Archive](https://archive.org/), so you can download [it here](https://www.dropbox.com/s/igsaopb8rur3r8t/NE2K_driver.fdd?dl=0).

Network setting:

1. Install the NE2K driver.
2. Shut down. Under Boot Order, make sure the hard drive is the first device, and add `devices.net.force_adapter_type=rtl` to the boot flags.
3. Set the shared network, then reboot.
4. Open HostManager.app. Under Local, choose "use local domain only".
5. Assign one of the available IPs in your local network, then reboot the machine.
6. Add a name server in `/etc/resolv.conf`.

[![Parallels boot flags setting](https://3.bp.blogspot.com/-N38_2lnWt0c/WcNqP8o6ScI/AAAAAAAA3h4/RDinEaRdDGkXw6N-CtImAc9i36iA8lKPgCLcBGAs/s640/Screen%2BShot%2B2017-09-21%2Bat%2B12.28.29%2BAM.png)](https://3.bp.blogspot.com/-N38_2lnWt0c/WcNqP8o6ScI/AAAAAAAA3h4/RDinEaRdDGkXw6N-CtImAc9i36iA8lKPgCLcBGAs/s1600/Screen%2BShot%2B2017-09-21%2Bat%2B12.28.29%2BAM.png)

[![HostManager local domain setting](https://2.bp.blogspot.com/-O2Elf37ptRM/WcNpDnvg1cI/AAAAAAAA3hs/oGUrXdcKEu4HeaDqXmBjfLUPe0Gwu5-GQCLcBGAs/s640/Parallels%2BPicture.png)](https://2.bp.blogspot.com/-O2Elf37ptRM/WcNpDnvg1cI/AAAAAAAA3hs/oGUrXdcKEu4HeaDqXmBjfLUPe0Gwu5-GQCLcBGAs/s1600/Parallels%2BPicture.png)

Installing the developer tools: there is a good video on YouTube.

The next step is to write some Objective-C code using [this book](http://www.nextcomputers.org/NeXTfiles/Docs/Software/OPENSTEP/802-2110.pdf) and to download some applications from <http://www.nextcomputers.org/NeXTfiles/>.

## References

- <http://openstep.bfx.re/>
- <http://www.nextcomputers.org/docs/FAQ-OpenStepOnEmulators.pdf>
- <https://forum.parallels.com/threads/openstep-4-2-driver-thread.3408/>
- <http://www.nextcomputers.org/forums/viewtopic.php?t=2992>
- <https://forum.parallels.com/threads/older-nic-emulation-before-gigabit.110324/>
