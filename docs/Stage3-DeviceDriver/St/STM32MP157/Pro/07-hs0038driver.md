# HS0038红外模块实验说明



## 1.硬件介绍

**模块原理图及资料**：`irda.zip`。

## 2.硬件与开发板连接的方法

STM32MP157先断电，按下图所示，将模块插在扩展板的GPIO0，将扩展板插在底板上。

注意：为了防止用户接错方向，模块和扩展板都有一条长白线，连接时需要模块上的白线和扩展板的白线在同一侧。

注意：hs0038模块中的如下图接线。

![hs0038](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/St/STM32MP157/Pro/07-hs0038driver_hs0038.png)

## 3.修改设备树

100ASK_STM32MP157_PRO的设备树目录为：	

`100ask_stm32mp157_pro-sdk/Linux-5.4/arch/arm/boot/dts/stm32mp157c-100ask-512d-lcd-v1.dts`

进入内核目录

```
cd ~/100ask_stm32mp157_pro-sdk/Linux-5.4
```

修改设备树dts

```
vi arch/arm/boot/dts/stm32mp157c-100ask-512d-lcd-v1.dts
```

增加如下内容，

```
   hs0038 {  /* for stm32mp157 */
        compatible = "100ask,hs0038";
        gpios = <&gpioc 3 GPIO_ACTIVE_HIGH>;
    };
```

![dts](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/St/STM32MP157/Pro/07-hs0038driver_dts.png)

保存退出后编译设备树

```
make dtbs
```

将生成的stm32mp157c-100ask-512d-lcd-v1.dtb拷贝到nfs_rootfs目录中

```
cp arch/arm/boot/dts/stm32mp157c-100ask-512d-lcd-v1.dtb ~/nfs_rootfs/
```

## 4.编译程序

程序位于source文件夹中。

修改Makefile文件，将下图中Makefile文件中的KERN_DIR修改为开发板内核目录，如使用的是百问网提供的Ubuntu镜像目录设置为：

```
/home/book/100ask_stm32mp157_pro-sdk/Linux-5.4
```

![Makefile](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/St/STM32MP157/Pro/07-hs0038driver_Makefile.png)

输入make，编译程序。

![make](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/St/STM32MP157/Pro/07-hs0038driver_make.png)

将生成的驱动程序hs0038_drv.ko和应用程序hs0038_test_input拷贝到网络文件系统中，输入

```
cp hs0038_drv.ko hs0038_test_input ~/nfs_rootfs/
```



## 5.开发板上机实验

在保证开发板和Ubuntu可以ping通的情况下，将开发板挂载网络文件系统，输入：

```
 mount -t nfs -o nolock,vers=3 192.168.5.11:/home/book/nfs_rootfs /mnt
```

注意上面的192.168.5.11需要修改为自己的Ubuntu的ip，具体操作可参考嵌入式Linux应用开发完全手册第2篇的第七章。

拷贝mnt目录下的设备树到boot目录，输入

```
cp /mnt/stm32mp157c-100ask-512d-lcd-v1.dtb /boot/
```

将mnt目录下的驱动程序和可执行程序拷贝出来

```
cp /mnt/hs0038_drv.ko ~
cp /mnt/hs0038_test_input ~
```

重启开发板

```
reboot
```

重启完成后，开始安装驱动，输入

```
insmod hs0038_drv.ko
```

测试程序：

```
./hs0038_test_input /dev/input/event0
```

使用红外遥控器，按下按键的实验现象：

![Experimental-phenomenon](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/St/STM32MP157/Pro/07-hs0038driver_Experimental-phenomenon.png)