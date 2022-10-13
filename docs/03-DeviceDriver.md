# Linux设备驱动简述(Introduction)
!!! note "驱动开发说明"
    此部分主要涉及模块驱动开发，适配我们的所有开发板 及相应模块，我们会提前将

## IMX6ULL-Pro开发步骤
### 步骤一
> 获取开发板配套的完整虚拟机，我们的虚拟机提前帮大家配置好了各种环境 包含vscode 示例工程源码 各种依赖工具包 中文输入法等，大家可以直接解压运行就可以开始着手开发。<br>
> 不同的板子因为 硬件差异 内核版本差异 工具链版本差异 库的支持不同 而提供专门的虚拟机系统镜像，请大家一定要根据自己的板子型号去获取对应的虚拟机。<br>

* 百度网盘链接:链接：https://pan.baidu.com/s/1cXwhwPMAY1wZ_XGWMtg_vw?pwd=kjnh   
* 奶牛快传链接:


### 步骤二

{==
    注意：整个网站里面每个板子对应的虚拟机都是同一个，只是在不同的页面都有介绍，如果您在其它部分页面下载了虚拟机系统镜像，则无需再这个页面获取，直接跳过 此步骤即可。
==}

> 使用VMware运行 步骤一获取到的开发板配套虚拟机镜像。

1.首先下载vmware虚拟机工具

使用浏览器打开网址 https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html 参考下图箭头所示，点击下载安装 Windows版本的VMware Workstation ，点击 DOWNLOAD NOW 即可开始下载。

![](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/vmwareworkstation_download_001.png)

2.下载完成后，只需要根据软件提示 一步步默认安装就可以，等待完成。 


注意：最后可能会提示输入密钥，自行根据版本 百度搜索即可。 


### 步骤三

> 配置虚拟机开发环境，和开发板互相通信。<br>
> 这一步决定了开发板是否可以和ubuntu通信,将你的程序上传至开发板执行。对于IMX6ULL开发板 STM32MP157开发板 直接通过USB转网卡设备，来实现 ubuntu与开发板互相通信，具体操作步骤如下，注意：USB网卡需要使用我们提供的专用网卡才可以，其它型号不一定支持。

1.ubuntu系统启动，将USB网卡设备通过Vmware挂载至ubuntu系统内。
![u18vm_configNet01](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Stage2-Components/u18vm_configNet01.jpg)
2.ubuntu内设置USB网卡静态IP地址
![u18vm_configNet01](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Stage2-Components/u18vm_configNet02.jpg)

![u18vm_configNet01](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Stage2-Components/u18vm_configNet03.jpg)

![u18vm_configNet01](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Stage2-Components/u18vm_configNet04.jpg)

![u18vm_configNet01](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Stage2-Components/u18vm_configNet05.jpg)


3.开发板内设置网卡静态IP地址并互相通信验证

![IMX6ULL-Pro_Login01](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Stage2-Components/IMX6ULL-Pro_Login01.jpg)

![IMX6ULL-Pro_Login02](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Stage2-Components/IMX6ULL-Pro_Login02.jpg)


![IMX6ULL-Pro_Login02](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Stage2-Components/IMX6ULL-Pro_Login03.jpg)

如果你觉得每次启动手动设置静态IP很麻烦 可以手动编辑 `/etc/network/interfaces `文件增加，也可以直接在开发板执行如下命令 来自动设置网卡信息。

```shell
mv /etc/network/interfaces ~/
cat << EOF > /etc/network/interfaces
auto lo
iface lo inet loopback
auto eth0
iface eth0 inet static
    address 192.168.5.9
    netmask 255.255.255.0
    gateway 192.168.5.1
EOF
```

### 步骤四
> 上传程序到开发板
参考步骤三，已经可以让ubuntu和开发板互相通信了，接下来我们可以用多种方式传输文件到我们的开发板内，可以是 开发板挂载ubuntu nfs目录，也可以是ssh scp传输等等。

----
* 使用scp方式传输<br>
> Linux scp 命令用于 Linux 之间复制文件和目录。scp 是 secure copy 的缩写, scp 是 linux 系统下基于 ssh 登陆进行安全的远程文件拷贝命令。

我们已经知道开发板的IP地址是 192.168.5.9 接下来我在ubuntu下创建一个 1.txt 并通过 scp在ubuntu上 上传到开发板。 
如下示例，使用echo创建一个内容是4个数字1的1.txt文件，然后通过scp上传到开发板的/root目录下。

```shell
book@ubuntu1804:~$ echo 11111 > 1.txt
book@ubuntu1804:~$ scp 1.txt  root@192.168.5.9:/root
1.txt                                                100%    6     1.4KB/s   00:00    
book@ubuntu1804:~$ 
```

开发板内查看上传到/root目录下的1.txt文件
```shell
[root@100ask:~]# ls /root/1.txt
/root/1.txt
[root@100ask:~]# cat /root/1.txt
11111
[root@100ask:~]#
```

更多用法可参考： https://www.runoob.com/linux/linux-comm-scp.html

----
* 开发板使用nfs网络文件系统方式
> NFS即网络文件系统，允许开发板直接通过网络挂载PC机中的文件夹。下面介绍在ubuntu上的NFS服务安装和配置。

首先，执行以下命令安装NFS服务：
```shell
book@100ask:~$ sudo apt-get install nfs-kernel-server
```
然后编辑/etc/exports文件，添加NFS服务导出的工作目录：
```shell
book@100ask:~$ sudo gedit /etc/exports
```
添加NFS目录：下面以/home/book/rootfs为例，将其添加到/etc/exports文件中, 如下：
```shell
/home/book/nfs_rootfs  *(rw,nohide,insecure,no_subtree_check,async,no_root_squash)
```
最后，重启NFS服务:
```shell
book@100ask:~$ sudo service nfs-kernel-server restart
```
如果一切正常，可以在ubuntu中测试NFS服务：
```shell
book@100ask:~$ sudo  mount  -t  nfs  127.0.0.1:/home/book/nfs_rootfs    /mnt
```

开发板端进入系统后，确保可以和ubuntu网络互通，之后执行如下命令进行挂载ubuntu的nfs网络文件系统。如下所示，将nfs文件系统挂载到开发板的/mnt目录下，之后进入 /mnt目录下，就可以看到ubuntu下里面的文件，这也就是实现了文件互传的效果。
```shell
[root@100ask:~]# mount -t nfs -o nolock 192.168.5.11:/home/book/nfs_rootfs /mnt
[root@100ask:~]# cd /mnt
```

### 步骤五
> 编译内核驱动模块 并上传开发板执行。想要开发一个硬件配套的设备驱动及测试程序，我们需要先得到开发板配套的内核源码并使用配套的工具链进行编译，编译完成后，才可以开始编译开发板配套的内核模块，最后上传到开发板内，运行。 

1.编译内核源码<br>
&nbsp;&nbsp; 内核源码 交叉编译工具链 我们已经提前帮您存放在 虚拟机镜像里，您只需要在这一步 进入内核源码 编译一下内核就可以。
鼠标在桌面 右键 打开一个 Terminal 终端，cd 进入到100ask_imx6ull-sdk/Linux-4.9.88 目录下 执行如下命令，等待内核编译完成即可。

![imx6ull-ldd0](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Stage2-Components/imx6ull-ldd0.jpg)

![imx6ull-ldd0](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Stage2-Components/imx6ull-ldd01.jpg)

```bash
book@ubuntu1804:~$ cd 100ask_imx6ull-sdk/Linux-4.9.88/
book@ubuntu1804:~/100ask_imx6ull-sdk/Linux-4.9.88$ make 100ask_imx6ull_defconfig
book@ubuntu1804:~/100ask_imx6ull-sdk/Linux-4.9.88$ make zImage -j8
```
&nbsp;&nbsp; 编译完成后，就可以进行下一步开始内核驱动开发了。

2.获取内核模块示例<br>
&nbsp;&nbsp; 这部分为演示操作，我们使用 一个 hello word 驱动示例程序作为演示，其中程序存放在 https://github.com/100askTeam/Stage3-DeviceDriver/tree/master/IMX6ULL/hello_drv 目录内，我们需要在ubuntu下获取此工程，然后进行编译，
&nbsp;&nbsp;首先clone此仓库，clone下来后，进入到Stage3-DeviceDriver/IMX6ULL/hello_drv/ 目录内，可以看到我们提前准备好的模块示例，因为内核目录 工具链都提前配好了，并且是一致的，可以直接执行 make命令进行编译,参考下述命令。

```bash
book@ubuntu1804:~$ git clone https://github.com/100askTeam/Stage3-DeviceDriver.git
Cloning into 'Stage3-DeviceDriver'...
remote: Enumerating objects: 417, done.
remote: Counting objects: 100% (121/121), done.
remote: Compressing objects: 100% (106/106), done.
remote: Total 417 (delta 33), reused 93 (delta 15), pack-reused 296
Receiving objects: 100% (417/417), 58.74 MiB | 2.25 MiB/s, done.
Resolving deltas: 100% (126/126), done.
book@ubuntu1804:~$ cd Stage3-DeviceDriver/IMX6ULL/hello_drv/
book@ubuntu1804:~/Stage3-DeviceDriver/IMX6ULL/hello_drv$ ls
hello_drv_0.jpg  hello_drv_1.jpg  hello_drv.c  hello_drv_test.c  Makefile  readme.md
book@ubuntu1804:~/Stage3-DeviceDriver/IMX6ULL/hello_drv$
```
![imx6ull-ldd03](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Stage2-Components/imx6ull-ldd02.jpg)

3.编译内核模块<br>
&nbsp;&nbsp;上一小节我们已经clone并进入了helloword驱动示例源码，并且已经编译过内核源码，配置好Makefile里面的kernel所在路径 交叉编译工具链等所有必要环境，接下来只需要在内核源码目录下直接执行 `make`命令开始编译。
![imx6ull-ldd03](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Stage2-Components/imx6ull-ldd03.jpg)
```bash
book@ubuntu1804:~/Stage3-DeviceDriver/IMX6ULL/hello_drv$ make 
make -C /home/book/100ask_imx6ull-sdk/Linux-4.9.88 M=`pwd` modules 
make[1]: Entering directory '/home/book/100ask_imx6ull-sdk/Linux-4.9.88'
  CC [M]  /home/book/Stage3-DeviceDriver/IMX6ULL/hello_drv/hello_drv.o
  Building modules, stage 2.
  MODPOST 1 modules
  CC      /home/book/Stage3-DeviceDriver/IMX6ULL/hello_drv/hello_drv.mod.o
  LD [M]  /home/book/Stage3-DeviceDriver/IMX6ULL/hello_drv/hello_drv.ko
make[1]: Leaving directory '/home/book/100ask_imx6ull-sdk/Linux-4.9.88'
arm-buildroot-linux-gnueabihf-gcc -o hello_drv_test hello_drv_test.c 
book@ubuntu1804:~/Stage3-DeviceDriver/IMX6ULL/hello_drv$ ls
hello_drv_0.jpg  hello_drv.ko     hello_drv.o       Makefile        readme.md
hello_drv_1.jpg  hello_drv.mod.c  hello_drv_test    modules.order
hello_drv.c      hello_drv.mod.o  hello_drv_test.c  Module.symvers
book@ubuntu1804:~/Stage3-DeviceDriver/IMX6ULL/hello_drv$ 

```
&nbsp;&nbsp;编译完成后，输入了一个 hello_drv.ko helldrv驱动模块文件，以及 hello_drv_test 驱动测试示例，接下来需要把这两个程序直接上传至开发板内，然后执行。


4.上传至开发板并执行<br>
&nbsp;&nbsp;上面 步骤四 我们介绍了两种传输文件到开发板的方式，这里我们使用开发板挂载 ubuntu nfs网络文件系统方式传输文件。
&nbsp;&nbsp;首先确保 上述 步骤三 已经没有问题，接下来 将 我们编译生成的驱动程序 `hello_drv.ko` `hello_drv_test`拷贝到 配置好的nfs目录下,在 hell_drv目录下执行 cp hello_drv.ko hello_drv_test ~/nfs_rootfs/ 目录即可。
```bash
book@ubuntu1804:~/Stage3-DeviceDriver/IMX6ULL/hello_drv$ cp hello_drv.ko hello_drv_test ~/nfs_rootfs/
book@ubuntu1804:~/Stage3-DeviceDriver/IMX6ULL/hello_drv$ ls ~/nfs_rootfs/
hello_drv.ko  hello_drv_test
```
![imx6ull-ldd04](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Stage2-Components/imx6ull-ldd04.jpg)

&nbsp;&nbsp;之后切换到 IMX6ULL-Pro开发板终端界面，登陆开发板系统，确保可以和ubuntu网络互通，然后执行 `mount -t nfs -o nolock 192.168.5.11:/home/book/nfs_rootfs /mnt` 命令，在开发板上挂载ubuntu nfs文件系统，挂载成功后，可以直接进入到 开发板的 /mnt目录下，执行安装驱动模块命令 `insmod hello_drv.ko` 命令，最后执行测试程序 `hello_drv_test`来测试驱动。

``` bash
[root@100ask:~]# ping 192.168.5.11
PING 192.168.5.11 (192.168.5.11): 56 data bytes
64 bytes from 192.168.5.11: seq=0 ttl=64 time=4.277 ms
^C
--- 192.168.5.11 ping statistics ---
1 packets transmitted, 1 packets received, 0% packet loss
round-trip min/avg/max = 4.277/4.277/4.277 ms
[root@100ask:~]# mount -t nfs -o nolock 192.168.5.11:/home/book/nfs_rootfs /mnt
[root@100ask:~]# cd /mnt/
[root@100ask:/mnt]# ls
hello_drv.ko  hello_drv_test
[root@100ask:/mnt]# insmod hello_drv.ko
[10604.815020] hello_drv: loading out-of-tree module taints kernel.
[root@100ask:/mnt]# ls /dev/hello
/dev/hello
[root@100ask:/mnt]# chmod +x hello_drv_test
[root@100ask:/mnt]# ./hello_drv_test
Usage: ./hello_drv_test -w <string>
       ./hello_drv_test -r
[root@100ask:/mnt]# ./hello_drv_test -w 100ask
[root@100ask:/mnt]# ./hello_drv_test -r
APP read : 100ask
[root@100ask:/mnt]#


```

![imx6ull-ldd05](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Stage2-Components/imx6ull-ldd05.jpg)

至此，我们就完整的在6ULL上运行了一个 驱动模块 以及测试程序了，更多设备驱动模块的开发示例 请大家访问 [设备驱动开发](/Stage3-DeviceDriver/Nxp/IMX6ULL/Pro/01-leddriver/)章节，**6ULLPro** 前缀的文章，里面有非常丰富的示例 给到大家。

## STM32MP157-Pro开发步骤
编写中........

## 支持的设备驱动示例
> 绿色√号表示已完成，灰色 √ 表示未完成。
### NXP开发板
- [x] IMX6ULL-Pro开发板
    * [x] [GPIO-LED灯示例驱动](Stage3-DeviceDriver/Nxp/IMX6ULL/Pro/01-leddriver/)
    * [x] [GPIO-Key按键实验示例](Stage3-DeviceDriver/Nxp/IMX6ULL/Pro/02-keydriver/)
    * [x] [GPIO-sr501人体红外传感器](Stage3-DeviceDriver/Nxp/IMX6ULL/Pro/03-sr501driver/)
    * [x] [GPIO-SR04超声波测距模块](Stage3-DeviceDriver/Nxp/IMX6ULL/Pro/04-SR04driver/)
    * [x] [GPIO-DS18B20温度模块](Stage3-DeviceDriver/Nxp/IMX6ULL/Pro/06-ds18b20driver/)
    * [x] [GPIO-DHT11温湿度模块](Stage3-DeviceDriver/Nxp/IMX6ULL/Pro/05-dht11driver/)
    * [x] [GPIO-IRDA红外遥控模块](Stage3-DeviceDriver/Nxp/IMX6ULL/Pro/07-hs0038driver/)
    * [x] [I2C-AT24C02存储模块](Stage3-DeviceDriver/Nxp/IMX6ULL/Pro/08-at24c02driver/)
    * [x] [SPI-OLED屏模块](Stage3-DeviceDriver/Nxp/IMX6ULL/Pro/09-oleddriver/)


### ST开发板

- [x] STM32MP157-Pro开发板
    * [x] [GPIO-LED灯示例驱动](Stage3-DeviceDriver/St/STM32MP157/Pro/01-leddriver/)
    * [x] [GPIO-Key按键实验示例](Stage3-DeviceDriver/St/STM32MP157/Pro/02-keydriver/)
    * [x] [GPIO-sr501人体红外传感器](Stage3-DeviceDriver/St/STM32MP157/Pro/03-sr501driver/)
    * [x] [GPIO-SR04超声波测距模块](Stage3-DeviceDriver/St/STM32MP157/Pro/04-SR04driver/)
    * [x] [GPIO-DS18B20温度模块](Stage3-DeviceDriver/St/STM32MP157/Pro/06-ds18b20driver/)
    * [x] [GPIO-DHT11温湿度模块](Stage3-DeviceDriver/St/STM32MP157/Pro/05-dht11driver/)
    * [x] [GPIO-IRDA红外遥控模块](Stage3-DeviceDriver/St/STM32MP157/Pro/07-hs0038driver/)
    * [x] [I2C-AT24C02存储模块](Stage3-DeviceDriver/St/STM32MP157/Pro/08-at24c02driver/)
    * [x] [SPI-OLED屏模块](Stage3-DeviceDriver/St/STM32MP157/Pro/09-oleddriver/)


## 如何参与共享？
* Linux驱动：https://github.com/100askTeam/Stage3-DeviceDriver
* 交流社区：https://forums.100ask.net/c/elinuxdev/23
