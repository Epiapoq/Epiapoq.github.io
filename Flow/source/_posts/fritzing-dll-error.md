---
title: Fritzing's ssleay.dll error
date: 2021-08-20 12:03:13
author: Epiapoq
tags:
    - Fritzing
    - STM32F10xxx
    - Embedded System
---
 
 ![modified adapter](adapter_modified.png)
 <!-- more -->
 
 <font size=4>When designing "pulse width measurement" lab of stm3210x, I used a ttl-to-usb module to connect mcu with pc. The module is the same one which is used to upload program to a diy arduino board(designed by one of my hard working student). The core chip of the module is CH340G. But same as the similar pruducts on market, this mudule alse dose not lead out a reset pin. Say, when uploading, we must press the reset button on the Arduino board at the proper instant of time. NOT CONVENIENT. So I try to draw a modified circuit in fritzing to solve the problem.</font>

![the only one adapter(CH340G) supports reset(DTR pin) I've ever seen, but can't find in China](adapter.png)

<font size=4>But when I was opening fritzing, popuped an error.</font>

![ssleay.dll error](error.png)

<font size=4>Then I found the path in the error dialogue box, and copied "ssleay32.dll" and "libeay32.dll" to fritzing's folder. Solved.</font>