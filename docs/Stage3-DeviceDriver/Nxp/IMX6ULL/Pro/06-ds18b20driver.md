# DS18B20温度模块驱动实验



## 1.硬件介绍

**模块原理图及资料**：网盘开发板配套资料“05_Hardware (原理图)/Extend_modules/ds18b20.zip”。



## 2.硬件与开发板连接方法

DS18B20也使用的是“1-Wire单总线”，只通过一条数据线传输数据，既要控制器发送数据给芯片，又要通过芯片发送数据给控制器，是双向传输数据。

DS18B20的硬件设计电路与前面的DHT11基本一致，原理图如下：

<img src="06-ds18b20driver_hardware.jpg" alt="hardware" style="zoom:50%;" />

IMX6ULL先断电，按下图所示，将模块插在扩展板的GPIO0，将扩展板插在底板上。

**注意**：为了防止用户接错方向，模块和扩展板都有一条长白线，连接时需要模块上的白线和扩展板的白线在同一侧。

<img src="06-ds18b20driver_ds18b20_module.jpg" alt="ds18b20_module" style="zoom:50%;" />





## 3.修改设备树

在设备树的根节点下添加如下信息：

```
   ds18b20 {  /* for imx6ull */
        compatible = "100ask,ds18b20";
        gpios = <&gpio4 19 GPIO_ACTIVE_HIGH>;
        /* test-gpios = <&gpio4 23 GPIO_ACTIVE_HIGH>; */
    };
```



## 4.编译设备树

```
book@100ask:~/100ask_imx6ull-sdk/Linux-4.9.88$ make dtbs
book@100ask:~/100ask_imx6ull-sdk/Linux-4.9.88$ cp arch/arm/boot/dts/100ask_imx6ull-14x14.dtb ~/nfs_rootfs/
```

![make_dtbs](06-ds18b20driver_make_dtbs.jpg)

把设备树拷贝到开发板上运行：

```
[root@100ask:/mnt]# cp 100ask_imx6ull-14x14.dtb /boot
[root@100ask:/mnt]# reboot
```





## 5.查看生成的设备树节点信息

reboot 使用新的设备树重新启动之后正常情况下会在开发板的“/proc/device-tree”目录下生成"ds18b20"设备树节点。



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
	$(CROSS_COMPILE)gcc -o ds18b20_test ds18b20_test.c
clean:
	make -C $(KERN_DIR) M=`pwd` modules clean
	rm -rf modules.order  ds18b20_test

# 参考内核源码drivers/char/ipmi/Makefile
# 要想把a.c, b.c编译成ab.ko, 可以这样指定:
# ab-y := a.o b.o
# obj-m += ab.o

obj-m += ds18b20_drv.o
```

在Ubuntu下，执行编译命令`make`，并拷贝到开发板上运行：

```
cp ds18b20_test *.ko ~/nfs_rootfs/
```





## 7.加载驱动模块并进行测试

```
[root@100ask:/mnt]# insmod ds18b20_drv.ko
[root@100ask:/mnt]# ./ds18b20_test /dev/myds18b20
```

![test](06-ds18b20driver_test.jpg)









