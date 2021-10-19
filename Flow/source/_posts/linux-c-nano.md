---
title: Linux C Programming -- nano editor
date: 2021-10-19 13:02:51
author: Epiapoq
tags:
    - Linux
    - C Programming
---

![nano editor](nano.png)
 <!-- more -->

 <font size=4>
 
 To program using C in linux, first of all, install Ubuntu in Windows. (https://docs.microsoft.com/zh-cn/windows/wsl/install-manual)

Then replace with domestic mirror source.

Next, install "gdb".

![Debug using gdb](gdb.png)

Last change the default settings of "nano" editor.

i. change tabsize: edit "nanorc" file (.\rootfs\etc\nanorc)

![change tabsize from 8 to 4](nanorc.png)

ii. change color of comment (.\rootfs\usr\share\nano\c.nanorc)

![change color of comment](c.nanorc.png)

Recommend two books:

A good Introductory book of C (linux)

![Linux C编程一站式学习](linux_c.png)

Another good Introductory book of C (8051, but not only for 8051)

![时间触发嵌入式系统设计模式](8051_c.png)

</font>