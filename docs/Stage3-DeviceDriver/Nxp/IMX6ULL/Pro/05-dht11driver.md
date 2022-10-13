# DHT11温湿度模块驱动实验

* 工程所在仓库：https://github.com/100askTeam/Stage3-DeviceDriver
* 默认情况此仓库已经提前给您放在 配套虚拟机 /home/book目录下。只需要进入 `/home/book/Stage3-DeviceDriver/IMX6ULL/05_dht11`路径 就可以开始您的开发。

## 1.硬件介绍

DHT11模块软件设计：

DHT11的硬件电路比较简单，核心要点就是 主机发给DHT11的命令格式和DHT11返回的数据格式。



## 2.硬件与开发板连接方法

和IRDA模块的电路基本一致，主机通过一条数据线与DH11连接，主机通过这条线发命令给DHT11，DHT11再通过这条线把数据发送给主机。

![connect](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/05-dht11driver_connect.jpg)



IMX6ULL先断电，按下图所示，将模块插在扩展板的GPIO0，将扩展板插在底板上。

**注意**：为了防止用户接错方向，模块和扩展板都有一条长白线，连接时需要模块上的白线和扩展板的白线在同一侧。

![line_connect](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/05-dht11driver_line_connect.jpg)





## 3.修改设备树

来到这个目录输入如下指令：

```
cd ~/100ask_imx6ull-sdk/Linux-4.9.88/arch/arm/boot/dts
vi 100ask_imx6ull-14x14.dts
```



①在`/`根节点下添加如下信息：

![devicetree](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/05-dht11driver_devicetree.jpg)







## 4.编译设备树

```
book@100ask:~/100ask_imx6ull-sdk/Linux-4.9.88$ make dtbs
book@100ask:~/100ask_imx6ull-sdk/Linux-4.9.88$ cp arch/arm/boot/dts/100ask_imx6ull-14x14.dtb ~/nfs_rootfs/
```

![make_dtbs](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/05-dht11driver_make_dtbs.jpg)

把设备树拷贝到开发板上运行：

```
[root@100ask:/mnt]# cp 100ask_imx6ull-14x14.dtb /boot
[root@100ask:/mnt]# reboot
```





## 5.查看生成的设备树节点信息

reboot 使用新的设备树重新启动之后正常情况下会在开发板的“/proc/device-tree”目录下生成“dht11”设备树节点。

```
ls /proc/device-tree
```



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
	$(CROSS_COMPILE)gcc -o dht11_test dht11_test.c
clean:
	make -C $(KERN_DIR) M=`pwd` modules clean
	rm -rf modules.order  dht11_test

# 参考内核源码drivers/char/ipmi/Makefile
# 要想把a.c, b.c编译成ab.ko, 可以这样指定:
# ab-y := a.o b.o
# obj-m += ab.o

obj-m += dht11_drv.o
```

![make](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/05-dht11driver_make.jpg)

把生成的`dht11_drv.ko`和`dht11_test`拷贝到NFS网络文件系统

执行命令：

```
book@100ask:~/IMX6ULL/05_dht11$ cp dht11_test *.ko ~/nfs_rootfs/
```



## 7.加载驱动模块并进行测试

```
[root@100ask:/mnt]# insmod dht11_drv.ko
[root@100ask:/mnt]# ./dht11_test /dev/mydht11
```

实验结果如下：

![dht11_test](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Nxp/IMX6ULL/Pro/05-dht11driver_dht11_test.jpg)









