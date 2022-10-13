## EEPROM模块驱动实验
* 工程所在仓库：https://github.com/100askTeam/Stage3-DeviceDriver
* 默认情况此仓库已经提前给您放在 配套虚拟机 /home/book目录下。只需要进入 `/home/book/Stage3-DeviceDriver/IMX6ULL/08_at24c02`路径 就可以开始您的开发。
> ### AT24C02简介
>
> AT24C02是基于I2C总线的存储器件，由于接口方便，体积小，数据掉电不丢失等特点，在仪器仪表及工业自动化控制中得到大量的应用。
>
> 百问网提供的EEPROM模块使用的就是AT24C02，使用8位地址，存储容量为2K bit，即2048bit = 256*8bit = 256 Byte, 其中它被分为32页，每页8Byte。



## 1.硬件介绍

EEPROM模块的原理图如下：

![datasheet](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/08-at24c02driver_datasheet.jpg)



## 2.硬件与开发板连接方法

IMX6ULL先断电，按下图所示，将模块插在扩展板的I2C_A，将扩展板插在底板上。

**注意**：为了防止用户接错方向，模块和扩展板都有一条长白线，连接时需要模块上的白线和扩展板的白线在同一侧。

**注意**：本实验中，AT24C02模块要插到I2C_A接口，如下图所示。

<img src="https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/08-at24c02driver_connect.jpg" alt="connect" style="zoom:50%;" />



## 3.修改设备树

```
book@100ask:~/100ask_imx6ull-sdk/Linux-4.9.88/$ vi arch/arm/boot/dts/100ask_imx6ull-14x14.dts
```

在`&i2c1`节点处加上如下信息：

![devicetree](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/08-at24c02driver_devicetree.jpg)





## 4.编译设备树

![make_dtbs](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/08-at24c02driver_make_dtbs.jpg)

把设备树拷贝到开发板上运行：

```
[root@100ask:/mnt]# cp 100ask_imx6ull-14x14.dtb /boot
[root@100ask:/mnt]# reboot
```



## 5.查看生成的设备树节点信息

reboot 使用新的设备树重新启动之后正常情况下会在开发板的“/proc/device-tree”目录下生成"at4c02"设备树节点。



## 6.Makefile说明

```makefile

# 1. 使用不同的开发板内核时, 一定要修改KERN_DIR
# 2. KERN_DIR中的内核要事先配置、编译, 为了能编译内核, 要先设置下列环境变量:
# 2.1 ARCH,          比如: export ARCH=arm64
# 2.2 CROSS_COMPILE, 比如: export CROSS_COMPILE=aarch64-linux-gnu-
# 2.3 PATH,          比如: export PATH=$PATH:/home/book/100ask_roc-rk3399-pc/ToolChain-6.3.1/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu/bin 
# 注意: 不同的开发板不同的编译器上述3个环境变量不一定相同,
#       请参考各开发板的高级用户使用手册

#KERN_DIR =  /home/book/100ask_stm32mp157_pro-sdk/Linux-5.4
KERN_DIR =  /home/book/100ask_imx6ull-sdk/Linux-4.9.88

all:
	make -C $(KERN_DIR) M=`pwd` modules 
	$(CROSS_COMPILE)gcc -o at24c02_test at24c02_test.c
clean:
	make -C $(KERN_DIR) M=`pwd` modules clean
	rm -rf modules.order  at24c02_test

# 参考内核源码drivers/char/ipmi/Makefile
# 要想把a.c, b.c编译成ab.ko, 可以这样指定:
# ab-y := a.o b.o
# obj-m += ab.o

obj-m += at24c02_drv.o
```



## 7.加载驱动模块并进行测试

![test](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/08-at24c02driver_test.jpg)











