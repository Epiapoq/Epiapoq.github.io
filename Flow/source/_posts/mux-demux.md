---
title: Explain Data Routing -- A Digital Design Method 
date: 2021-04-04 22:22:17
author: Epiapoq
tags:
    - Arduino
    - Proteus
    - Digital Design
    - Curriculum Integration 
---

![](mux_demux.gif)

<font size=4>In this post, I try to use digital logic to explain the basic concept of data routing.</font><!-- more -->

![Data Routing](mux_demux.png)

<font size=4>As we can see, in the circuit above, there are two transmitters 1&2 and two receivers 1&2. Relationship's as follows.</font>

```markdown
Transmitter_1 -- Receiver_1
Transmitter_2 -- Receiver_2
```

<font size=4>Because there are four bits outputs for each transmmiter, we need at least `8-bit` lines when connect them seperately.</font>

<font size=4>But if we use MUX(74HC157) and Decoder(74HC139) to connect them, we just need `4-bit` lines. (Here we can ignore all the controlling bus, such as the lines from the decoder, because we can do that when the amount of transmitters and receivers increases to a huge number.) The two Transceiver Pairs will share the outputs of the MUX, if we can handle the sequence of the clock well.</font>

<font size=4>Of couse we also need a DEMUX to distribute the datas to the right destination. Unlike the MUX in the circuit, which is hardware, I use External Inerrupt(INT0) of Arduino UNO to respond to the controlling signal of the Decoder. I call it a soft demux.(I can't find a proper 74 series demux which has double four-bit outputs.)</font>

<font size=4>The Virtual Serial Port Pairs Map is:</font>

```markdown
COM1--COM5
COM2--COM6
```

<font size=4>COM1: Port of Transmitter_1</font>

<font size=4>COM2: Port of Transmitter_2</font>

<font size=4>COM5: Port of Arduino IDE Serial Monitor_1</font>

<font size=4>COM6: Port of Arduino IDE Serial Monitor_2</font>

<font size=4>tip: only four serial port supported in Proteus</font>

<font size=4>Finally, the concepts of switching, routing, multiplexing and demultiplexing, whatever you name them, `SPDT` is their prototype in my opinion.</font>

![spdt](spdt.png)

![routing](data_routing.jpg)