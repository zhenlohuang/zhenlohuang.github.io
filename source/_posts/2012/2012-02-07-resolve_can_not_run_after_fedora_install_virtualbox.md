---
layout: post
title: '解决Fedora安装Virtualbox后无法运行'
date: 2012-2-7
wordpress_id: 535
permalink: /archives/resolve_can_not_run_after_fedora_install_virtualbox.html
categories: [Linux]
tags: [Fedora]
keywords: "Fedora"
description: 
comments: true
---

    
在Fedora下安装了Virtualbox，发现运行时出现以下问题：     
>Kernel driver not installed (rc=-1908)     
>The VirtualBox Linux kernel driver (vboxdrv) is either not loaded or there is a permission problem with /dev/vboxdrv. Please reinstall the kernel module by executing
>‘/etc/init.d/vboxdrv setup’
>as root. Users of Ubuntu, Fedora or Mandriva should install the DKMS package first. This package keeps track of Linux kernel changes and recompiles the vboxdrv kernel module if necessary.


然后以root身份运行/etc/init.d/vboxdrv setup
结果提示：
>Stopping VirtualBox kernel modules [确定]
>Uninstalling old VirtualBox DKMS kernel modules [确定]
>Trying to register the VirtualBox kernel modules using DKMSError! Bad return status for module build on kernel: 3.2.3-2.fc16.i686 (i686)
>Consult /var/lib/dkms/vboxhost/4.0.8/build/make.log for more information.[失败]
>(Failed, trying without DKMS)
>Recompiling VirtualBox kernel modules [失败]
>(Look at /var/log/vbox-install.log to find out what went wrong)


解决方法：
先尝试：

```
sudo yum install -y kernel-headers kernel-devel dkms gcc
sudo /etc/init.d/vboxdrv setup
```

如果执行此操作，仍然出现上述错误。可以参考下面
安装 PAE 包， sudo yum install kernel-PAE-devel 。
完成后再执行 sudo/etc/init.d/vboxdrv setup

