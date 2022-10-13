# SR04超声波测距模块驱动实验


* 工程所在仓库：https://github.com/100askTeam/Stage3-DeviceDriver
* 默认情况此仓库已经提前给您放在 配套虚拟机 /home/book目录下。只需要进入 `/home/book/Stage3-DeviceDriver/IMX6ULL/04_SR04`路径 就可以开始您的开发。


## 1.硬件介绍

模块原理图及资料：网盘开发板配套资料“05_Hardware (原理图)/Extend_modules/超声波.zip”。



## 2.硬件与开发板连接的方法

IMX6ULL先断电，按下图所示，将模块插在扩展板的GPIO0，将扩展板插在底板上。

注意：为了防止用户接错方向，模块和扩展板都有一条长白线，连接时需要模块上的白线和扩展板的白线在同一侧。

注意：注意SR04模块中的红线、黑线、绿线、黄线的位置，如下图接线。

<img src="https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/04-SR04driver_hardware.jpg" alt="hardware" style="zoom: 33%;" />





## 3.修改设备树

来到这个目录输入如下指令：

```
cd ~/100ask_imx6ull-sdk/Linux-4.9.88/arch/arm/boot/dts
vi 100ask_imx6ull-14x14.dts
```



①在`/`根节点下添加如下信息：

![devicetree](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/04-SR04driver_devicetree.jpg)



## 4.编译设备树

```
book@100ask:~/100ask_imx6ull-sdk/Linux-4.9.88$ make dtbs
book@100ask:~/100ask_imx6ull-sdk/Linux-4.9.88$ cp arch/arm/boot/dts/100ask_imx6ull-14x14.dtb ~/nfs_rootfs/
```

![make](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/04-SR04driver_make.jpg)

把设备树拷贝到开发板上运行：

```
[root@100ask:/mnt]# cp 100ask_imx6ull-14x14.dtb /boot
[root@100ask:/mnt]# reboot
```



## 5.查看生成的设备树节点信息

reboot 使用新的设备树重新启动之后正常情况下会在开发板的“/proc/device-tree”目录下生成“sr501”设备树节点。





## 6.Makefile说明

```makefile
# 1. 使用不同的开发板内核时, 一定要修改KERN_DIR
# 2. KERN_DIR中的内核要事先配置、编译, 为了能编译内核, 要先设置下列环境变量:
# 2.1 ARCH,          比如: export ARCH=arm64
# 2.2 CROSS_COMPILE, 比如: export CROSS_COMPILE=aarch64-linux-gnu-
# 2.3 PATH,          比如: export PATH=$PATH:/home/book/100ask_roc-rk3399-pc/ToolChain-6.3.1/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu/bin 
# 注意: 不同的开发板不同的编译器上述3个环境变量不一定相同,
#       请参考各开发板的高级用户使用手册

KERN_DIR = /home/book/100ask_imx6ull-sdk/Linux-4.9.88

all:
	make -C $(KERN_DIR) M=`pwd` modules 
	$(CROSS_COMPILE)gcc -o sr04_test sr04_test.c
clean:
	make -C $(KERN_DIR) M=`pwd` modules clean
	rm -rf modules.order  sr04_test

# 参考内核源码drivers/char/ipmi/Makefile
# 要想把a.c, b.c编译成ab.ko, 可以这样指定:
# ab-y := a.o b.o
# obj-m += ab.o

obj-m += sr04_drv.o
```

在Ubuntu下，执行编译命令`make`

![make_ok](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/04-SR04driver_make_ok.jpg)

把生成的`sr04_drv.ko`和`sr04_test`拷贝到NFS网络文件系统





## 7.加载驱动模块并进行测试

加载命令：

```
insmod sr04_drv.ko
```



实验现象：

打印出距离

![test](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/04-SR04driver_test.jpg)

