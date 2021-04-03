---
title: How to connect COMPIM (Proteus) with Serial Monitor (Arduino IDE) 
date: 2021-04-02 19:01:17
author: Epiapoq
tags: 
    - Arduino
    - Proteus
    - Curriculum Integration 
---

![](6.gif)

<font size=4>This is a tutorial about how to connect Proteus with Arduino IDE by serial port. Take BCD Adder for an instance.</font><!-- more -->

<font size=4>1. Install [virtual serial port driver](https://www.eltima.com/vspd-post-download.html) for creating virtual serial port pair.</font>

![fig.1 VSPD](1.png)

![fig.2 Instrument Manager](2.png)

<font size=4>2. Pick "COMPIM" component in Proteus and setup baud rate and port number, so does "VIRTUAL TERMINAL".</font>

![fig.3 COMPIM](3.png)

![fig.4 VIRTUAL TERMINAL](4.png)

<font size=4>3. Setup Serial Monitor in Arduino IDE.</font>

![fig.5 Serial Monitor](5.png)

<font size=4>Now run simulation in Proteus and input addend in Serial Monitor in Arduino IDE. The data will be transmitted and received by COM1/COM2.</font>

![fig.6 Result](6.gif)

---

<font color=888888>_Demo codes:_</font>

```c++
/*  
    LAB2: Addends generator for FPGA BCD adder
    Programming & Hardware By Epiapoq
    Started 29-03-2021
    Modified 30-03-2021
    Version 1.0
    Reference:
    https://github.com/whatuptkhere/paralleloslam/blob/master/Paralleloslam.ino
*/

int firstByte = 0; // the first addend from serial port
int secondByte = 0; // the second addend from serial port
int sum = 0; // the sum
int cb = 0; // check bit, which is used to check the number of the incomming addend

/* each addend of BCD adder consists of only 4 bits, so here 
just define the lower nibble of the firstByte or secondByte */
const int firstByte_0 = 4; // LSB of first addend
const int firstByte_1 = 5;
const int firstByte_2 = 6;
const int firstByte_3 = 7; // MSB of first addend
const int secondByte_0 = 8; // LSB of second addend
const int secondByte_1 = 9;
const int secondByte_2 = 10;
const int secondByte_3 = 11; // MSB of second addend

void setup() {
    Serial.begin(9600); // open serial port, set data rate to 9600 bps
    pinMode(firstByte_0, OUTPUT);
    pinMode(firstByte_1, OUTPUT);
    pinMode(firstByte_2, OUTPUT);
    pinMode(firstByte_3, OUTPUT);
    pinMode(secondByte_0, OUTPUT);
    pinMode(secondByte_1, OUTPUT);
    pinMode(secondByte_2, OUTPUT);
    pinMode(secondByte_3, OUTPUT);
}

void loop() {
    if (Serial.available() > 0 && (cb == 0)) { // read the first addend
        firstByte = Serial.read() - 48;
        
        /* serial in from monitor and parallel out to FPGA */
        digitalWrite(firstByte_0, firstByte & 0x01); // send LSB of first addend
        digitalWrite(firstByte_1, firstByte & 0x02);
        digitalWrite(firstByte_2, firstByte & 0x04);
        digitalWrite(firstByte_3, firstByte & 0x08);
        
        Serial.print(firstByte);
        Serial.print('+');
        cb = 1;
    }
    else if ((Serial.available() > 0) && (cb == 1)) { // read the second addend
        secondByte = Serial.read() - 48;

        /* serial in from monitor and parallel out to FPGA */
        digitalWrite(secondByte_0, secondByte & 0x01);
        digitalWrite(secondByte_1, secondByte & 0x02);
        digitalWrite(secondByte_2, secondByte & 0x04);
        digitalWrite(secondByte_3, secondByte & 0x08);
        
        Serial.print(secondByte);
        Serial.print('=');
        sum = firstByte + secondByte;
        Serial.print(sum); 
        Serial.println('\n');
        cb = 0;
    }
}
```