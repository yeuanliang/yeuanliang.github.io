+++
title = "Dual Boot"
date = "2024-09-20T14:14:20+08:00"
description = "dual boot openbsd with ubuntu"
tags = ["journal","cs",]
+++

## Ubuntu 和 OpenBSD 双系统启动

我在2018年1月初购买的HP Zhan66 G1 Pro笔记本有两块硬盘：250G的NVMe SSD和500G的SATA，前两天在SSD上装了Ubuntu 24.04，在SATA上装了OpenBSD 7.5，之后笔记本开机后会直接进入Ubuntu，通过BIOS的引导菜单可以进入到OpenBSD。今天上午我学会了用Grub2生成启动菜单，这样无需进入BIOS即可选择要启动的系统。具体步骤如下：

1、进入Ubuntu系统后，用编辑器打开`/etc/default/grub`，将`GRUB_TIMEOUT_STYLE`的值改为`menu`,`GRUB_TIMEOUT`的值改为`5`(菜单显示时间，大于0的整数均可)，保存后执行`update-grub`，`reboot`；

2、看到启动菜单后，按`c`进入Grub Terminal，执行`ls`查看所有磁盘，然后对每个显示的磁盘执行`ls (hdX, msdosY)`（X、Y为具体数字），输出为`bsd bsd.booted bsd.rd ...`的便是OpenBSD所在的磁盘，记录磁盘信息，`reboot`后再次进入Ubuntu；

3、用编辑器打开`/etc/grub.d/40_custom`，在文件的最后添加以下内容（不要修改原有内容）：
```
menuentry "OpenBSD" {
    kopenbsd -r sd0a (hd0,msdos4)/bsd #sd0a (hd0,msdos4)需根据个人情况修改
}
```
保存后再执行`update-grub`，`reboot`，重启后就能在启动菜单中看到OpenBSD了，选中即可进入到OpenBSD系统。

附注：1、我在安装OpenBSD和Ubuntu时均未选择全盘加密，并且OpenBSD安装时磁盘分区用的是MBR。2、在安装Ubuntu时也打算用MBR分区，让系统安装在整个磁盘上，但是安装失败，后来在硬盘上先分了一个ESP区，剩余空间分为`/`才安装成功。

参考资料

* [OpenBSD: Full encryption and dual boot with gnu/linux](https://astro-gr.org/openbsd-full-encryption-with-dual-boot/)
* [OpenBSD Workstation for the People](https://www.tumfatig.net/2024/openbsd-workstation-for-the-people/)
* [Running OpenBSD 7.5 on you laptop is really hard(not)](https://www.k58.uk/openbsd.html)
