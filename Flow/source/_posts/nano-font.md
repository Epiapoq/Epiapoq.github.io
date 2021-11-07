---
title: 如何更改nano的默认字体（How to set nano default font）
date: 2021-11-06 23:35:18
author: Epiapoq
tags:
    - Linux
    - C Programming
---

![Create CodePage](regedit.png)

<font size=4>在Linux Shell（或PowerShell、cmd）中启动nano，即便更改了字体的默认值（见下图），在重新打开后仍旧会回到系统初始设定，要解决这个问题需要更改注册表，即在注册表中上图所在位置添加CodePage。(参考：https://zhuanlan.zhihu.com/p/265192449)</font>

![Attribute Setting](attributes.png)