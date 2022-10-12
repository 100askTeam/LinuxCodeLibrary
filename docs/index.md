# 站点简述(Introduction)

!!! abstract "目标"

    当你想实现某个功能时，先到百问网的代码中心，看看有没有参考的代码。百问网代码中心，汇集各类代码，加快开发速度:<br>
    涉及单片机、RTOS、Linux 囊括从上到下全栈开发：应用组件、C程序、驱动程序、系统(bootloader和内核等)<br>
    提供教程链接：如果我们编写了某个模块的教程，会提供链接<br>
    教程适用于只想快速做出成果的同学，更适用于毕设，我们正在不断丰富完善，支持更多硬件芯片及模块，提供更丰富的demo.





## 如何阅读此站？

{==

建议使用PC端浏览器访问，分辨率1080p为最佳阅读。

==}

### 嵌入式开发流程简述
  在进入嵌入式Linux开发之前，我们需要先了解一下嵌入式系统的开发框架以及流程，如下图所示，左侧为我们常用的PC电脑主机，右侧为嵌入式系统，因为嵌入式芯片的性能内存存储条件等原因 无法直接在嵌入式芯片上进行程序开发，此时我们就需要借助性能强大的PC主机，一般为X86电脑，通过交叉编译的方式生产 嵌入式芯片可以运行的程序，在这里，我们一般叫PC主机为Host端，对于我们的嵌入式开发板，我们一般成为Target端。我们需要在Host端使用针对于Target端芯片架构生成的工具链进行交叉编译，最后将输出的可执行文件存放至Target(目标开发板)内运行。
![eLinuxHostAndTarget](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/eLinuxHostAndTarget.jpg)

  通过上图可以看出，实际的嵌入式开发是通过两部分组成的，那么对于嵌入式Linux设备开发也是如此。通过Host端的交叉编译工具链编译 Target(目标开发板)的程序，然后放在Target开发板上运行。
![cross-gcc-sandwich](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/cross-gcc-sandwich.png)  

  既然Target(目标开发板)上运行的程序是通过Host(PC主机)端交叉编译工具链生成，那么交叉编译工具链又是从哪里而来？这里就涉及到了一个BuildSystem(构建系统)的概念，所有的程序都不可能是直接凭空出现，而是通过编程开发，最后再编译得出。下面这张图演示了，交叉编译工具链的生成以及如何使用交叉编译工具链编译生成一个可以在Target(目标开发板)上运行的c++程序。这里主要告诉大家，所有的程序都是从源码编译得出，不管是交叉编译工具链还是 Target上运行的程序。
![gcc-cross-compiler](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/gcc-cross-compiler.png)

* 如果看了上述的介绍还是不太理解 嵌入式的开发流程框架可以看我们之前专门录制的一套基于buildroot开发的嵌入式基础知识科普类视频教程。
<iframe width="800px" height="600px" src="//player.bilibili.com/player.html?aid=897646032&bvid=BV1VN4y137Tf&cid=753046575&page=9" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>


### 代码库使用步骤
!!! note
    详细的各个部分开发步骤请查看 左侧导航  [LinuxC基础](/01-LinuxCprogrammers/)  [Linux组件开发](/02-Components/) [Linux设备驱动开发](/03-DeviceDriver/) [Linux系统开发](04-System/) 页面，在页面内会有详细的各部分开发流程介绍。

``` mermaid
graph LR
A[0.准备硬件]-->B[1.获取Ubuntu虚拟机系统]-->2.配置Host开发环境-->3.获取Target工程示例-->4.Host端编译开发-->5.上传至Target开发板运行
```


### 网站资源汇总

我们将本站所有的资源通过思维导图的方式罗列出来，大家可以根据自己的需要直接点击最右侧的链接，即可跳转到代码工程区域。


```markmap
---
markmap:
  colorFreezeLevel: 4
---
# 所有示例
## LinuxC基础
### Makefile基础
- [Demo](https://github.com/gera2ld/markmap)
### GCC基础
- [Demo](https://github.com/gera2ld/markmap)
### C语言基础
- [Demo](https://github.com/gera2ld/markmap)
### 信号处理
- [Demo](https://github.com/gera2ld/markmap)

## 组件示例
### LVGL显示组件
- [Demo](https://github.com/gera2ld/markmap)

### SDL显示组件
- [Demo](https://github.com/gera2ld/markmap)
### QT5显示组件
- [Demo](https://github.com/gera2ld/markmap)

### Wayland显示组件
- [Demo](https://github.com/gera2ld/markmap)
### WPA网络组件
- [Demo](https://github.com/gera2ld/markmap)
### BlueZ网络组件
- [Demo](https://github.com/gera2ld/markmap)

### MQTT组件
- [Demo](https://github.com/gera2ld/markmap)

### MJPG-Stream网络组件
- [Demo](https://github.com/gera2ld/markmap)

### PPPoE网络组件
- [Demo](https://github.com/gera2ld/markmap)


### libusb库
- [Demo](https://github.com/gera2ld/markmap)

### i2c-tools工具
- [Demo](https://github.com/gera2ld/markmap)

### gstreamer组件


## 驱动示例
### Pinctrl子系统
- [NXP](https://github.com/gera2ld/markmap)
- [ST](https://github.com/gera2ld/markmap)
### GPIO子系统
- [LED灯](https://markmap.js.org/)
- [KEY按键](https://github.com/gera2ld/markmap)

### I2C子系统
- [SSD1306](https://markmap.js.org/)
- [GT911触摸屏](https://github.com/gera2ld/markmap)

### Interrupt子系统

### SPI子系统
- [SPI显示屏](https://markmap.js.org/)
- [ADX135六轴](https://github.com/gera2ld/markmap)

### Input子系统
- [按键输入](https://github.com/gera2ld/markmap)

### UART子系统
- [RS485](https://github.com/gera2ld/markmap)
- [UART](https://github.com/gera2ld/markmap)

### PCI/PCI-e子系统
- [M2设备](https://github.com/gera2ld/markmap)
- [PCI-E网卡](https://github.com/gera2ld/markmap)
### LCD驱动
- [RGB-LCD](https://github.com/gera2ld/markmap)
- [MIPI-LCD](https://github.com/gera2ld/markmap)


## 系统开发
### Pinctrl子系统
- [NXP](https://github.com/gera2ld/markmap)
- [ST](https://github.com/gera2ld/markmap)
```

## 如何参与编辑？

* 我们所有的文档使用makrdown进行编写，存放在github仓库https://github.com/100askTeam/LinuxCodeLibrary 内，可以直接点击每个页面右上角的 🖊 箭头直接编辑修改。
* 对于代码示例等工程，全部都根据不同的使用场景存放在不同的git仓库内。

1. Linux基础：https://github.com/100askTeam/Stage1-Applications
2. 组件：https://github.com/100askTeam/Stage2-Components
3. 驱动：https://github.com/100askTeam/Stage3-DeviceDriver
4. 系统：https://github.com/100askTeam/Stage4-System

> 愿景：我们正在不断地支持更多的硬件模块，代码示例，加入到这个站点内，如果您感兴趣，欢迎加入。

* 讨论交流：https://forums.100ask.net/c/elinuxdev/23


## 您将获得什么？
快速实现您需要实现的功能，我们提供专门的开发板硬件，配套的模块硬件，设备驱动 系统源码SDK工程源码，常用组件开发示例，以及LinuxC基础，让大家可以在最短的时间内实现自己需要的功能。


## 关于开源协议
  此页面使用了开源的Mkdoc文档框架，文档站点托管在GitHub上，每个页面都会有编辑按钮，大家可以一起参与编辑或者提问改进此文档。
* 图片引用: https://preshing.com/20141119/how-to-build-a-gcc-cross-compiler/