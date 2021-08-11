---
title: Modify STM32F10x_StdPeriph_Lib_V3.5.0 to Add GPIO_ToggleBits Fuction
date: 2021-08-11 11:37:59
author: Epiapoq
tags:
    - STM32F10xxx
    - Embedded System
---

![GPIO_ToggleBits](main.png)
<!-- more -->

<font size=4>When designing the Blinking experiment, I found that it's not very convenient to drive the leds on/off in a brief way. The original method is to define a LEDx(ON/OFF) function to drive the leds on and off seperately, Because there is no toggle-bit function in STM32F10x_StdPeriph_Lib_V3.5.0. I guess that's why in Fire's manual, a self defined function is used.</font>

![Fire's Method](fire's.png)

<font size=4>Finally I noticed that there is "GPIO_ToggleBits" Function in "stm32f4xx_gpio.c" of stm32f4_dsp_stdperiph_lib_v1.8.0. So I tried to transplant it into "stm32f10x_gpio.c" and "stm32f10x.h", and it works. Method follows:</font>

![Function in stm32f10x_gpio.c](trans_1.png)

![Declaration in stm32f10x_gpio.h](trans_2.png)

![Declaration in bsp_led.h](trans_3.png)

![Simulation in Proteus](simulation.gif)

<font size=4>Not real time running, just like always.¯\\\_(ツ)_/¯</font>
