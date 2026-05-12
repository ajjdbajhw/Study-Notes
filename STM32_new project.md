Stm32开发

2025年9月12日
12:57

       Stm32开发方式主要有三类，基于标准库（库函数），基于寄存器，基于HAL库的方式。基于寄存器的方式，是用程序直接配置寄存器，达到想要的功能，类似于51单片机。这种功能最底层、最直接，效率高，但是stm32的结构复杂、寄存器太多，这种方法不方便。     基于库函数的方式使用st公司封装好的函数，直接对寄存器进行配置。
      通过固件库中的Libary 文件中的库函数来新建工程，Project是官方提供的工程示例和模板，可供参考使用。Utiities是测试程序，测试stm32开发板能否正常使用。Release是版本说明，stm32f10x是使用手册，包括库函数的使用。
    其中片上资源（外设）丰富，Peripheral，通过程序配置外设，来达到我们想要的功能。例如：NVIC systick  都是内核里的外设，其余的都是内核外的外设。

     STM32系列微控制器基于ARM Cortex-M内核，具有丰富的外设资源。这些外设可以大大扩展单片机的功能，使其适用于各种应用场景。以下是一些常见的STM32片上外设及其功能的概述：
1.GPIO (General Purpose Input/Output) - 通用输入输出端口
功能：数字输入和输出，可配置为推挽、开漏、上拉/下拉等模式，用于控制外部设备或读取数字信号。

2.USART/UART (Universal Synchronous/Asynchronous Receiver/Transmitter) - 通用同步/异步串行接收/发送器
功能：串行通信，支持全双工异步通信（UART）或半双工同步通信（USART），常用于与电脑、传感器或其他微控制器通信。

3.SPI (Serial Peripheral Interface) - 串行外设接口
功能：高速全双工同步串行通信，用于与FLASH、SD卡、显示屏、传感器等外设通信。

4.I2C (Inter-Integrated Circuit) - 集成电路总线
功能：多主多从串行通信总线，用于连接低速外设，如EEPROM、传感器、RTC等。

5.ADC (Analog to Digital Converter) - 模数转换器
功能：将模拟信号（如电压）转换为数字值，用于读取传感器模拟输出（如光敏电阻、电位器）。

6.DAC (Digital to Analog Converter) - 数模转换器
功能：将数字值转换为模拟电压输出，用于生成模拟信号（如音频输出）。

7.TIM (Timer) - 定时器
功能：定时、计数、产生PWM波形、输入捕获、输出比较等，用于控制电机、测量频率、生成精确延时等。

8.DMA (Direct Memory Access) - 直接内存存取
功能：在外设和内存之间直接传输数据，无需CPU介入，可提高数据传输效率和减少CPU负担。

9.RTC (Real-Time Clock) - 实时时钟
功能：提供日历和时钟功能，即使在系统低功耗模式下也能运行，通常由备用电池供电。

10.Watchdog (IWDG/WWDG) - 看门狗定时器
功能：独立看门狗（IWDG）和窗口看门狗（WWDG），用于检测程序运行异常，防止系统死锁。

11.CAN (Controller Area Network) - 控制器局域网
功能：用于汽车和工业领域的可靠通信，支持多主网络。

12.USB (Universal Serial Bus) - 通用串行总线
功能：支持USB设备、主机或OTG功能，用于连接计算机或其他USB设备。

13.Ethernet MAC - 以太网媒体访问控制器
功能：支持以太网通信，用于网络连接。

14.SDIO (Secure Digital Input Output) - 安全数字输入输出接口
功能：用于连接SD卡、MMC卡等存储设备。

15.FSMC (Flexible Static Memory Controller) - 灵活静态存储器控制器
功能：用于扩展外部存储器，如SRAM、NOR Flash、LCD等。

16.CRC (Cyclic Redundancy Check) - 循环冗余校验
功能：硬件计算CRC，用于数据完整性检查。




Stm32新建工程
   (1) 先打开keil5，点击projiect   new uVision project，选择新建的文件夹，取个Project的通用工程名  （1）选择器件型号，STM32F103C8，OK
（2）工程不能直接使用，需要添加固件库，打开Libraries，CMSIS，CM3，DeviceSupport，ST，STM32F10x，startup，arm  之中的都是STM32的启动文件，STM32的程序就是从这些启动文件开始执行的。
（3）在工程文件中新建名为Start的文件，将arm中的启动文件全部移到Start文件中。
（4）接着回到STM32F10x 文件夹里，将其中的三个文件1. stm32f10x.h（stm32外设寄存器描述文件）   头文件stm32f10x.h包含了 STM32F10x系列微控制器的外设寄存器的描述，这个头文件定义了所有外设寄存器的结构体、地址映射以及寄存器位的定义。       2. system_stm32f10x.c     system_stm32f10x.h来配置时钟，stm32主频72HZ.
       将这三个文件也复制到Start文件下
（5）stm32是由内核和内核外围的设备一起完成的，且内核的寄存器描述和外围的寄存器描述文件不是在一起的，因此还需要添加外围寄存器描述文件。打开Libraries，CMSIS，CM3，CoreSupport，中的两个外围寄存器配置文件core_cm3.c   和core_cm3.h  一起移到Start启动文件下。
（6）添加启动文件，将keil5中的Souce Group 1 文件更名为Start，添加文件Start   
										
			

中的All file ，选择md.s的文件添加进来，然后将Start中的所有.c和.h文件添加进来，必须的且不能修改，所以这些文件都加了钥匙，只可读。
（7）最后，还需要 在工程选项里添加上这个文件夹的头文件路径，不然软件找不到.h文件，点击魔术棒，c/c++ 中的Include Paths，点击三个点，将Start的文件路径添加进来。
（8）创建main函数，在工程文件下创建User文件，在keil 中Target 1 右键Add Group…，将新建文件改名User，右键User  ，添加.c的新文件，名字为main，且路径要改成工程文件夹下的User。然后就可以在main中编写函数了，先写完框架，点编译，完成新建工程的建立。    注：函数最后要空一行，进行void的返回












 







（9）添加库函数，工程文件下新建Library的文件夹，打开固件库Libraries，STM32F10x_StdPeriph_Driver，src   其中，misc.c是内核的库函数，其余的都是内核外的外设库函数，全选，复制到Library文件夹下；然后再打开inc文件，里面都是库函数的头文件，全选，复制到Library文件夹下。
          回到keil软件，右键Target 1，添加组，改名Library，右键，添加文件，将刚才的文件全部添加进来，就完成了。
         对于库函数来说，还不能够直接使用，还需要再添加一个文件，打开固件库文件夹，打开Project，STM32Template，复制里面的三个文件，stm32f10x_conf.h     stm32f10x_it.c   stm32f10x_it.h   文件，第一个是用来配置库函数头文件的包含关系的，两个it文件是用来存放中断关系的。复制粘贴到User文件目录下，然后回到keil软件，把这三个文件添加到User目录下。
        打开stm32f10x.h文件，下滑到USE_STDPERIPH_DRIVER这个字符串，意思是如果你定义了这个字符串，下面的include conf.h语句才有效。
        因此要复制这个字符串到，工程选项里的c/c++的Define栏目里，这样才能包含保准外设库，也就是库函数。下方的头文件路径Include Paths中将User 和 Library文件路径也添加上。三个小箱子按钮可以改变左侧文件的顺序，将带钥匙的移到上方，不能修改。





扳手按钮，选择UTF-8，可避免中文乱码

魔术棒，DeBug ，选择ST-link，然后点击Settings

      选择Flash-Download，勾选上Reset and Run ，这样程序下载后会立马复位并执行，如果不设置这个，下载完程序后还需按下板子上的复位按钮，才能执行程序。
    之后就可以点击Download下载到板子上了。



