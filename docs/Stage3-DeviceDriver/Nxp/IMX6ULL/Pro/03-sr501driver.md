# sr501人体红外传感器实验说明

* 工程所在仓库：https://github.com/100askTeam/Stage3-DeviceDriver
* 默认情况此仓库已经提前给您放在 配套虚拟机 /home/book目录下。只需要进入 `/home/book/Stage3-DeviceDriver/IMX6ULL/03_sr501`路径 就可以开始您的开发。

## 1.硬件介绍

**模块原理图及资料**：网盘开发板配套资料`05_Hardware (原理图)/Extend_modules/人体红外感应.zip`。



## 2.硬件与开发板连接的方法

IMX6ULL先断电，按下图所示，将模块插在扩展板的GPIO0，将扩展板插在底板上。

注意：为了防止用户接错方向，模块和扩展板都有一条长白线，连接时需要模块上的白线和扩展板的白线在同一侧。

注意：注意SR501模块中红线、黑线、绿线的位置，如下图接线。

<img src="https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/03-sr501driver_sr501_hardware.jpg" alt="sr501_hardware" style="zoom: 33%;" />





## 3.修改设备树

来到这个目录输入如下指令：

```
cd ~/100ask_imx6ull-sdk/Linux-4.9.88/arch/arm/boot/dts
vi 100ask_imx6ull-14x14.dts
```



①在`iomuxc`子节点下添加如下信息：

![iomuxc](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/03-sr501driver_iomuxc.jpg)



②在`/`根节点下添加如下信息：

![device](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/03-sr501driver_device.jpg)



## 4.编译设备树

```
book@100ask:~/100ask_imx6ull-sdk/Linux-4.9.88$ make dtbs
book@100ask:~/100ask_imx6ull-sdk/Linux-4.9.88$ cp arch/arm/boot/dts/100ask_imx6ull-14x14.dtb ~/nfs_rootfs/
```

![make_dtbs](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/03-sr501driver_make_dtbs.jpg)

把设备树拷贝到开发板上运行：

```
[root@100ask:/mnt]# cp 100ask_imx6ull-14x14.dtb /boot
[root@100ask:/mnt]# reboot
```



## 5.查看生成的设备树节点信息

reboot 使用新的设备树重新启动之后正常情况下会在开发板的“/proc/device-tree”目录下生成“sr501”设备树节点。

![sr501](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/03-sr501driver_sr501.jpg)



## 6.Makefile说明

```makefile

# 1. 使用不同的开发板内核时, 一定要修改KERN_DIR
# 2. KERN_DIR中的内核要事先配置、编译, 为了能编译内核, 要先设置下列环境变量:
# 2.1 ARCH,          比如: export ARCH=arm64
# 2.2 CROSS_COMPILE, 比如: export CROSS_COMPILE=aarch64-linux-gnu-
# 2.3 PATH,          比如: export PATH=$PATH:/home/book/100ask_roc-rk3399-pc/ToolChain-6.3.1/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu/bin 
# 注意: 不同的开发板不同的编译器上述3个环境变量不一定相同,
#       请参考各开发板的高级用户使用手册

KERN_DIR =  /home/book/100ask_imx6ull-sdk/Linux-4.9.88

all:
	make -C $(KERN_DIR) M=`pwd` modules 
	$(CROSS_COMPILE)gcc -o sr501_test sr501_test.c
clean:
	make -C $(KERN_DIR) M=`pwd` modules clean
	rm -rf modules.order  sr501_test

# 参考内核源码drivers/char/ipmi/Makefile
# 要想把a.c, b.c编译成ab.ko, 可以这样指定:
# ab-y := a.o b.o
# obj-m += ab.o

obj-m += sr501_drv.o
```

在Ubuntu下，执行编译命令

![make](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/03-sr501driver_make.jpg)

把生成的`sr501_drv.ko`和`button_test`拷贝到NFS网络文件系统



## 7.加载驱动模块并进行测试

加载命令：

```
[root@100ask:/mnt]# insmod sr501_drv.ko
```



实验现象：

当有人靠近的时候，内核会有打印信息出现

![printk](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/03-sr501driver_printk.jpg)