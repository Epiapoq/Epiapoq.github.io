---
title: Professional Project Process -- Pro^3
date: 2021-07-28 13:53:23
author: Epiapoq
tags:
    - Pro^3
    - Digital Design
    - Curriculum Integration 
---

<font size=4>本文介绍了电气层（OSI Physical Layer）硬件接口实现电平匹配全过程——以FPGA+Arduino实验平台项目开发为例。</font><!-- more -->

![](step-mxo2.png)

<font size=4>2021-1学期给18级留学生讲授可编程逻辑器件课程期间，针对学生特点，以实验为主体，主要实验设备为Step-MXO2和Arduino Uno，通过二者之间的配合，极大丰富了该课程的内涵与外延，开发了如串行通信加法器、带频率显示的时钟分频器、DDS正弦信号发生器和FSK调制器等与通信概念密切相关的实验，体现了与传统“点灯”类可编程课程的显著不同。课程深受留学生欢迎，64学时课程自愿上成了96学时（每次2学时实验结束留学生都会自觉延长1学时以完成全部内容）。</font>

<font size=4>要实现Step-MXO2和Arduino Uno的连通，第一要务自然是满足二者输入输出之间的电平匹配，笔者将在此文中详细阐明整个设计流程，希冀能够同时解答部分学生对于PCB Layout如何转向其上层的硬件电路设计的疑问。</font>

<font size=4>1. Step-MXO2和Arduino Uno可否直连？</font>

![](interface.png)

<font size=4>短时间内或许不会互相伤害，但是逻辑上是不兼容的。这在数字电路课程中有过介绍。详见Digital Fundamentals 11th Edition by Thomas L. Floyd Chapter 15-1，不兼容的原因如下图（出自该书）所示。</font>

![3.3v CMOS](3.3v.png)

![5v TTL](5v.png)

<font size=4>由上面两图可见，兼容和不兼容皆有。兼容的一面在于：3.3v的CMOS输出电平，逻辑0的电平范围是（0-0.4v），逻辑1的电平范围是（2.4-3.3v），与此同时5v的TTL输入电平，逻辑0的电平范围是（0-0.8v），逻辑1的电平范围是（2-5v）。由于后者包含前者，是故前者可以直接驱动后者，也就是说Step-mxo2输出引脚可直连Arduino Uno输入引脚。不兼容的一面在于：5v的TTL输出电平，逻辑0的电平范围是（0-0.4v），逻辑1的电平范围是（2.4-5v），与此同时3.3v的CMOS输入电平，逻辑0的电平范围是（0-0.8v），逻辑1的电平范围是（2-3.3v）。由于后者不能完全包含前者，是故前者不可以直接驱动后者，也就是说Arduino Uno输出引脚直连Step-mxo2输入引脚时，高电平不能被后者识别。</font>

<font size=4>2. 上述理论放之四海而皆准吗？</font>

<font size=4>上述理论完全来自于教科书上的阐述，但在工程实践中尽信书不如无书，按图索骥不可取。在这里，理论的作用在于直觉上的提醒，告诉我们直连很有可能是行不通的（事实上也确实行不通），而在实践中要靠芯片手册说话，我们需要一个明确的答案。</font>

<font size=4>芯片生产商往往都会在官方网站上免费提供datasheet，首先我们找到Step-MXO2和Arduino Uno各自的核心芯片，分别为MachXO2和ATmega328P，上网搜索其官方链接下载查看。</font>

<font size=4>MachXO2: https://www.latticesemi.com/-/media/LatticeSemi/Documents/DataSheets/MachXO23/FPGA-DS-02056-3-7-MachXO2-Family-Data-Sheet.ashx?document_id=38834</font>

<font size=4>Atmega328P: http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-7810-Automotive-Microcontrollers-ATmega328P_Datasheet.pdf</font>

<font size=4>找出与输入输出电平相关的描述（其实也不是那么容易能找出来的，平时需要多看手册），如下：</font>

![MachXO2](datasheet_3.3v.png)

![ATmega328P](datasheet_5v.png)

<font size=4>显然这与上述教科书中的定义差异显著。但结论还是一样，不能完全兼容（具体计算过程参考上文，这里还有一点点瑕疵在于，datasheet中Machxo2输出1对应的电平最小值是VCCIO-0.4，即3.3v-0.4v=2.9v，而Atmega328P输入1的最小值是0.6VCC=3V，二者之间有0.1v的不匹配，但实操没有发现问题），进而需要思考如何设计电路实现匹配。</font>

<font size=4>3. 从不匹配到匹配如何实现？</font>

<font size=4>CMOS直接驱动TTL基本不存在问题，但反过来不行，不行的根本原因主要还是在于最大电压的不同，因此，如果能够将TTL最大电压5v降至3.3v，则很有可能解决不匹配的问题。最容易想到的办法就是电路原理里面学的串联分压，比如三个1kohm电阻串联，如下图所示（留学生上课时即采用此简易电路）。</font>

![](compatible.png)

<font size=4>4. 简单分压是不是就万事大吉了？</font>

<font size=4>工程上永远都在寻找一种折中，电路简单意味着设备复杂度低，好处是成本低，不足之处也是显而易见的，传输速率一定不高。原因在电子电路中已经学过了，CMOS的输入端存在容性，现在凭空多了三个电阻，形成了RC充放电一阶电路，其过渡过程会导致波形由方的变成圆的（自行脑补矩形波作为输入信号，RC电路中电容两端电压波形随输入信号频率变化而变化），速率越高越明显，直至由于C来不及充放电而使得信号湮灭。</font>

<font size=4>因此暑假期间组织了几位精干的学生在简易电阻分压的基础上采用TI专用芯片来实现电平匹配的同时又能满足信号传输速率的要求。</font>

<font size=4>74LVC4245: https://www.ti.com.cn/cn/lit/ds/symlink/sn74lvc4245a.pdf?ts=1627398220163&ref_url=https%253A%252F%252Fwww.ti.com.cn%252Fproduct%252Fcn%252FSN74LVC4245A%253FkeyMatch%253D%2526tisearch%253Dsearch-everything%2526usecase%253Dpartmatches</font>

![](74lvc4245.png)

<font size=4>5. 用上专用芯片是不是就完美了？</font>

<font size=4>电路设计没有完美的说法，还有待时间和实践的检验，学习也是一样，永无止境。</font>

<font size=4>6. 对以后的学习会有何帮助吗？</font>

<font size=4>至少当我们看到STM32F10x参考手册时，这种熟悉感会使我们更快的投入到新的学习中。</font>

![](stm32f10x.png)