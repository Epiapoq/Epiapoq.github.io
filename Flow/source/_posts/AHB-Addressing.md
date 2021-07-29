---
title: AHB何以不是起始于0x40018000？
date: 2021-07-29 16:15:29
author: Epiapoq
tags:
    - STM32F10xxx
    - Embedded System
---

![AHB在stm32f10x.h中的定义](AHB_stm32f10x.h.png)
<!-- more -->
<font size=4>在寄存器映射头文件stm32f10x.h中，官方定义了外设各总线的起始地址，其中AHB起始于0x40020000，各教程中也都默认此定义，不做更多说明。但事实上这里是会引起一点小歧义的。</font>

![stm32f10xxx架构图](SDIO_arch.png)

![AHB寄存器映射表](SDIO_addr.png)

<font size=4>由stm32f10xxx架构图可以看出，SDIO直连AHB，并不经由南北桥转接，因此SDIO理应包含在AHB中，而寄存器映射表也证实了这一点。显然，AHB实际起始于0x40018000。</font>

<font size=4>而stm32f10x.h之所以要将AHB定义成起始于0x40020000，猜测是为了便于记忆使然。没有找到官方佐证，唯一找到一位网友于我心有戚戚焉。https://www.programmersought.com/article/42371934487/</font>

<font size=4>由此，在stm32f10x.h中对于SDIO的定义就显得格外与众不同。</font>

![与众不同的SDIO](SDIO_stm32f10x.h.png)

<font size=4>教书切忌一知半解，自勉。</font>
