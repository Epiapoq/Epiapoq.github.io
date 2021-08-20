---
title: How to generate 4 channel PWM at the same time on nucleo -- REMAP
date: 2021-08-20 13:01:37
author: Epiapoq
tags:
    - Nucleo
    - STM32F10xxx
    - Embedded System
---

<font size=4>When configuring TIM3 as a PWM generater, CH1-CH4 are PA6, PA7， PB0, PB1. </font>

![four GPIO pins for 4 channels](4_channels.png)
<!-- more -->

<font size=4>Eventhough all the above four pins have been lead out on nucleo, 0 ohms resistors of PA6 and PA7 remain unsoldered by default. We must find another way to generate the other 2 PWM if we don't want to solder SB36 and SB39.</font> 

![PA6 and PA7 connected with SB36 and SB39](connections.png)

![SB36 and SB37 unsoldered with 0 ohms resistor by default](unsoldered.png)

<font size=4>Like FPGA, some pins(not all) of stm32f10x can be remapped to each other. We can use "fullremap" to remap CH1-CH4 to PC6-PC9 or "partialremap", only PA6-PA7 to PB4-PB5.(I also tried the latter, PB5 worked but PB4 still didn't. Don't know why.)</font> 

![TIM3_REMAP Table](table.png)

```c
RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC, ENABLE);
RCC_APB2PeriphClockCmd(RCC_APB2Periph_AFIO, ENABLE); // Enable both GPIOx and AFIO Clock before remapping
GPIO_PinRemapConfig(GPIO_FullRemap_TIM3, ENABLE); // FullRemap to PC6, PC7, PC8, PC9
```

<font size=4>Finally I got all PWM, but still don't understand why PB4 malfunctions. Maybe need read UM1724 again and again. Sugar cane is not sweet at both ends. Now that we enjoyed the convennience of finished hardware, we must accept the limitition of it and try our best to read the user's manual and rack our brains to find an alternative method.</font> 