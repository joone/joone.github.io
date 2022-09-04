---
title: 'Setting up NFS in OpenStep'
date: 2017-09-27T02:43:00.000-07:00
draft: false
aliases: [ "/2017/09/setting-up-nfs-in-openstep.html" ]
tags : [OpenStep]
---

If your server or host PC supports NFS,  you can set up shared directories for OpenStep. For example, if the shared folder is nfs://192.168.1.19/nfs/ftp, you can add the imported directory in NFSManager.app as follows:  

  
[![](https://1.bp.blogspot.com/-pljdigjFnEA/WctwzNYzjrI/AAAAAAAA8PE/j0LSMaQkw_wIM-7hfPgDuLfFnHoiaoWcwCLcBGAs/s640/Screen%2BShot%2B2017-09-27%2Bat%2B2.33.46%2BAM.png)](https://1.bp.blogspot.com/-pljdigjFnEA/WctwzNYzjrI/AAAAAAAA8PE/j0LSMaQkw_wIM-7hfPgDuLfFnHoiaoWcwCLcBGAs/s1600/Screen%2BShot%2B2017-09-27%2Bat%2B2.33.46%2BAM.png)

  
Run mount command in OpenStep to mount a remote directory as follows:```
$ mount -t 192.168.8.16:/mnt/openstep /coreonion 
```Reference  

*   [https://www.cyberciti.biz/faq/apple-mac-osx-nfs-mount-command-tutorial/](https://www.cyberciti.biz/faq/apple-mac-osx-nfs-mount-command-tutorial/)
*   [http://nextstep.onionmixer.net/wordpress/?p=139](http://nextstep.onionmixer.net/wordpress/?p=139)