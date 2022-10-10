# 系统管理

## apt命令

> 附加工具 使用 aptitude 图形化的软件包管理器。

```bash
[ubuntu@book:~]$ sudo apt update	//更新软件源包列表为最新状态
[ubuntu@book:~]$ sudo apt upgrade   //更新所有的包为最新状态
[ubuntu@book:~]$ sudo apt install <pkg>  //安装一个软件包，也可以是多个。
[ubuntu@book:~]$ sudo apt reinstall <pkg>  //重新安装一个软件包。
[ubuntu@book:~]$ sudo apt remove <pkg>   //移除一个软件包
[ubuntu@book:~]$ sudo apt-cache search <string> //apt源中搜索一个软件包
[ubuntu@book:~]$ sudo apt list  //列出我们已经安装的软件包
[ubuntu@book:~]$ sudo apt show <pkg> //查看某个软件包的详细信息。
```

* aptitude 图形化包管理界面，具体使用方法可以使用鼠标以及页面提示进行操作。

![image-20220423115915656](C:\Users\livel\Desktop\Linux常用命令.assets\image-20220423115915656-16506863579231.png)



## dpkg 命令

> 离线安装deb包的命令。

```bash
[ubuntu@book:~]$ sudo dpkg -i <pkg.deb>  //手动安装已经下载好的软件包
[ubuntu@book:~]$ sudo dpkg -r <pkg> 		//手动移除已经下载好的软件包
```



## snap命令

> 在线软件包管理命令

```bash
[ubuntu@book:~]$ sudo snap version //查看snap的版本状态
[ubuntu@book:~]$ sudo snap find "media player"  //搜索支持 media player的软件包
[ubuntu@book:~]$ sudo snap install vlc 		   //安装vlc软件
[ubuntu@book:~]$ sudo snap info vlc 			   //查看vlc软件的详细信息。
[ubuntu@book:~]$ sudo snap install --channel=edeg vlc  //安装测试版的vlc
[ubuntu@book:~]$ sudo snap switch  --channel=stable vlc  //切换vlc软件包为稳定版。
[ubuntu@book:~]$ snap list 					//查看已经安装过的软件包状态
```



## top命令

> 查看系统资源使用情况

```bash
[ubuntu@book:~]$ top
```

![image-20220423141512987](C:\Users\livel\Desktop\Linux常用命令.assets\image-20220423141512987.png)

## htop命令

```bash
[ubuntu@book:~]$ sudo apt install htop
[ubuntu@book:~]$ htop
```

![image-20220423141422517](C:\Users\livel\Desktop\Linux常用命令.assets\image-20220423141422517.png)

## uname 命令

> 显示系统信息

![image-20220423141635479](C:\Users\livel\Desktop\Linux常用命令.assets\image-20220423141635479.png)



## find命令

> 用于查找和搜索文件&目录。

```bash
[ubuntu@book:~]$ find  -name <file> 			 //在当前路径下搜索名为 file所有文件。
[ubuntu@book:~]$ find ~/Desktop -name test.txt //在Desktop目录下搜索 test.txt文件
[ubuntu@book:~]$ find ~/Desktop -name "*.txt" //在Desktop目录下搜索所有的后缀是 .txt文件.
[ubuntu@book:~]$ find . 					 //显示当前目录下所有的文件及目录。
[ubuntu@book:~]$ find  ~ -print > home.txt   //将~目录下的所有的文件文件夹列表重定向至 home.txt
[ubuntu@book:~]$ find /usr/bin/ -size  +1M 	 //列出/usr/bin/目录下所有大于1M的文件。
[ubuntu@book:~]$ find . -type f -atime +3    //列出当前路径下3天内被访问过的文件。
[ubuntu@book:~]$ find . -type f -amin +10    //列出当前路径下超过访问10分钟的文件。
[ubuntu@book:~]$ find ./ ! -name "*.txt"	 //列出当前路径下非后缀名为.txt的文件及文件夹。
```



## adduser命令

> 创建一个新的用户

```bas
[ubuntu@book:~]$ sudo adduer book  //新增一个名为book的用户
[ubuntu@book:~]$ su book			//切换到book用户，此时提示输入密码，需要输入book用户的密码。
[ubuntu@book:~]$ su ubuntu			//切换回ubuntu系统，输入ubuntu用户的密码。
```



## userdel命令

> 删除一个用户

```bash
[ubuntu@book:~]$ sudo userdel book 	//删除一个名为book的用户。
[ubuntu@book:~]$ sudo userdel -rf  book	//使用-rf删除book用的所有资源。
```



## passwd 

> 设置用户密码
```bash
[ubuntu@book:~]$ sudo passwd ubuntu   //重新设置Ubuntu用户密码。
[ubuntu@book:~]$ sudo passwd 		   //重新设置root用户密码。
```


## exit命令

> 退出当前打开的shell

```bash
[ubuntu@book:~]$ exit 
```



## reboot

> 用于重新启动计算机。

```bash
[ubuntu@book:~]$ sudo reboot 
[ubuntu@book:~]$ sudo reboot -n  //在重启之前不将数据缓存写入硬盘。
[ubuntu@book:~]$ sudo reboot -f	  //强制重启，类似于断电。
[ubuntu@book:~]$ sudo reboot -d   //虽然重启，但是不写入系统log日志中。
[ubuntu@book:~]$ sudo reboot -w   //不重启，但是写入系统重启装操作log日志中。
```

## poweroff

> 用于关闭计算机。

```bash
[ubuntu@book:~]$ sudo poweroff 
[ubuntu@book:~]$ sudo poweroff -n  //在关机之前不将数据缓存写入硬盘。
[ubuntu@book:~]$ sudo poweroff -f	  //强制关机，类似于断电。
[ubuntu@book:~]$ sudo poweroff -d   //虽然关机，但是不写入系统log日志中。
[ubuntu@book:~]$ sudo poweroff -w   //不关机，但是写入系统关机装操作log日志中。
```



## shutdown

> 关机操作，在关机之前会将关机信息传递给所有正在执行的程序。

```bash
[ubuntu@book:~]$ sudo shutdown -h now //立即关机
[ubuntu@book:~]$ sudo shutdown -t 10  //10秒后关机
[ubuntu@book:~]$ sudo shutdown -r now  //立即重启
[ubuntu@book:~]$ sudo shutdown -h 3  	//3分钟后关机
[ubuntu@book:~]$ sudo shutdown -c   	//中断正在关机的动作
```



## which命令

> 用于查找系统内文件所在位置

```bash
[ubuntu@book:~]$ which gdb  //查找gdb命令所在位置并显示出来。
```



## ps命令

> 显示当前系统正在运行进行的状态等信息。

```bash
[ubuntu@book:~]$ ps -aux  //查看详细的进程状态信息等。
[ubuntu@book:~]$ ps -ef | grep "ssh" //查看完整的进行路径及PID等，配合着grep查找我们需要的进程信息。
[ubuntu@book:~]$ ps -u root  //查看root用户的进程信息。
```



## clear命令

> 用于清屏操作，主要清理终端字符串。

```b
[ubuntu@book:~]$ clear
```



## su命令

> 用于切换用户身份，变更当前终端的用户。

```bash
[ubuntu@book:~]$ su root //切换到root用户，输入的是root用户名密码。
[ubuntu@book:~]$ su ubuntu //切换到ubuntu用户 输入的是Ubuntu用户密码。
```



## whoami命令

> whoami命令用于显示自身用户的名称

```bash
[ubuntu@book:~]$ whoami
```



## who命令

> 用于显示当前系统中有那些登录用户，以及登录用户的详细信息。

```bash
[ubuntu@book:~]$ who  		//显示当前登录到该系统的所有用户
[ubuntu@book:~]$ who  -H 	//显示标题栏
[ubuntu@book:~]$ who -T  -H 	//显示登录终端属性
[ubuntu@book:~]$ who -q		//以精简模式显示登录的用户状态

```





## date 命令

> 用于显示系统当前时间信息。

```b
[ubuntu@book:~]$ date
```



## dmesg

> 用于查看kernel日志信息命令。

```bash
[ubuntu@book:~]$ dmesg
```