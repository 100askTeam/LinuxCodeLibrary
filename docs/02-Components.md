# Linux组件简述(Introduction)

## 组件开发流程介绍
!!! note
    对于嵌入式Linux组件开发，主要包含 社区组件 库 等，借助强大的社区力量，去实现自己的项目，因为社区组件库等非常多，且有专门的使用说明，所以我们会找出一些常用的组件 库 给大家提供开发示例说明。

整个开发流程主要分为两部分，步骤1至步骤3，主要配置Host端开发环境，步骤4 5 为开发Target开发板程序并运行测试。

### 步骤一
> 获取开发板配套的完整虚拟机，我们的虚拟机提前帮大家配置好了各种环境 包含vscode 示例工程源码 各种依赖工具包 中文输入法等，大家可以直接解压运行就可以开始着手开发。<br>
> 不同的板子因为 硬件差异 内核版本差异 工具链版本差异 库的支持不同 而提供专门的虚拟机系统镜像，请大家一定要根据自己的板子型号去获取对应的虚拟机。<br>

#### IMX6ULL-Pro全功能开发板
* 百度网盘链接:链接：https://pan.baidu.com/s/1cXwhwPMAY1wZ_XGWMtg_vw?pwd=kjnh   
* 奶牛快传链接:


#### STM32MP157-Pro
* 百度网盘链接： 
* 奶牛快传链接：


### 步骤二
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
#### IMX6ULL-Pro开发板

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


#### STM32MP157-Pro开发板

待补充

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
> 运行应用程序并调试,通过前面的步骤 已经可以实现ubuntu和开发板互相通信传输文件了，下面演示在ubuntu下使用VSCODE编译HelloWrod 并在开发板上运行 调试！

1.获取hello-word示例模板，模板文件存放在git仓库里，可以参考下述命令在终端下执行获取工程，获取到以后，在hello-word目录下执行 `code .`命令就可以唤醒vscode进入开发界面，当然也可以点击ubuntu左侧 vscode图标 找到 hello-word 工程所在目录打开。
![runhellexample03](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Stage2-Components/runhellexample01.jpg)

```bash
book@ubuntu1804:~$ git clone https://github.com/100askTeam/Stage2-Components.git
Cloning into 'Stage2-Components'...
remote: Enumerating objects: 12, done.
remote: Counting objects: 100% (12/12), done.
remote: Compressing objects: 100% (10/10), done.
remote: Total 12 (delta 0), reused 12 (delta 0), pack-reused 0
Unpacking objects: 100% (12/12), done.
book@ubuntu1804:~$ cd Stage2-Components/
book@ubuntu1804:~/Stage2-Components$ cd hello-world/
book@ubuntu1804:~/Stage2-Components/hello-world$ code .
```

2.使用vscode打开工程以后可以看到如下主界面，其中红框处是hellowrd程序源码，左侧分别是 makefile vscode配置文件等。

![runhellexample03](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Stage2-Components/runhellexample02.jpg)

3. 如果需要编译此工程可以参考下图 点击vscode 左上菜单栏 找到  `Terminal 1`然后再弹出的子页面菜单项点击 `Run Build Task 2`来编译构建此工程，当然也可以使用 快捷键 ctrl + shift + b 。
![runhellexample03](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Stage2-Components/runhellexample03.jpg)

构建时可以看到 右下的  终端会话界面，里面详细的显示出来了编译的步骤等信息。
![runhellexample03](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Stage2-Components/runhellexample04.jpg)

也可以点击右侧的 红色框1 `bash`字符，切换到当前工程的shell终端命令界面，在里面进行其它操作，我们可以将编译好的文件 直接通过 scp nfs等方式传到开发板内，然后再开发板执行。
![runhellexample03](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Stage2-Components/runhellexample05.jpg)

4.如果你不想手动上传编译出来的目标程序，也可以参考下图执行 点击 vscode 左上菜单栏界面里，`Run 1`按钮，再弹出的子页面里面 点击 `Start Debugging 2`来自动进入调试界面。
![runhellexample03](https://cdn.staticaly.com/gh/DongshanPI/LinuxCodeLibrary-Photos@master/Stage2-Components/runhellexample06.jpg)

综上，完整演示了如何使用ubuntu系统vscode开发一个可以在开发板上运行的helloword程序，接下来就可以进入 [Linux组件开发](/Stage2-Components/AudioAndVideoApplications/)页面进行实际的工程开发了！

## 文档完成度

- [x] 音频与显示相关组件
- [x] 网络通信相关组件


## 如何参与编辑？
* Linux组件：https://github.com/100askTeam/Stage2-Components
* 交流社区：https://forums.100ask.net/c/elinuxdev/23
