# 磁盘操作



## fdisk命令

> fidsk是一个用来创建和维护磁盘设备分区的一个实用工具。

```bash
[ubuntu@book:~]$ fdisk -l   //列出当前系统所有的磁盘设备
[ubuntu@book:~]$ fdisk /dev/sdc //操作设备节点为 /dev/sdc的一个设备。
```

* p : 显示所有的分区。
* d: 删除分区。
* n: 创建一个新的分区。
* t : 更改分区类型。
* w: 保存修改并退出。
* m: 显示帮助信息。





## gparted

> 图形化分区操作工具。

```bash
[ubuntu@book:~]$ sudo apt install gparted
```

![image-20220423192823569](C:\Users\livel\Desktop\Linux常用命令.assets\image-20220423192823569-16507133054645.png)



## mkfs.fat

> 格式化分区为 fat32类型

```bash
[ubuntu@book:~]$ sudo mkfs.fat /dev/sdc1
```



## mkfs.ext2/3/4

> 格式化文件系统类型为 Linux EXT类型。

```bash
[ubuntu@book:~]$ sudo mkfs.ext4 /dev/sdc2
```



## mount命令

> 挂载系统外的磁盘分区或者目录等。

```ba
[ubuntu@book:~]$ sudo mount -t vfat /dev/sdc1 /mnt 
[ubuntu@book:~]$ sudo umont /mnt 
[ubuntu@book:~]$ sudo umont /dev/sdc1

[ubuntu@book:~]$ sudo mount -t ext4 /dev/sdc2 /mnt 
[ubuntu@book:~]$ sudo umont /mnt 
[ubuntu@book:~]$ sudo umont /dev/sdc2
```



## df命令

> 显示Linux上文件系统磁盘使用的占用情况。

```bash
[ubuntu@book:~]$ df  //显示文件系统的磁盘使用情况。
[ubuntu@book:~]$ df -h  //以易读的形式展示输出磁盘的使用情况
```

![image-20220423195254048](C:\Users\livel\Desktop\Linux常用命令.assets\image-20220423195254048.png)



## du命令

> 用于显示目录或文件大小。

```bash
[ubuntu@book:~]$ du 		//显示当前目录下的所有文件及目录大小。
[ubuntu@book:~]$ du -h   //以比较直观的方式显示所有文件及目录大小。
[ubuntu@book:~]$ du dir -h  //以比较直观方式显示dir目录大小。
[ubuntu@book:~]$ du  file1  //显示file文件大小。
[ubuntu@book:~]$ du  dir -h --max-depth=1 //以比较直观的方式显示dir目录并只显示目录深度为1级。
```



## dd命令

> dd命令用于数据的读取转换等操作。

```ba
[ubuntu@book:~]$ dd --help   //查看dd帮助命令。
[ubuntu@book:~]$ dd if=/dev/zero of=dd.img count=1024 bs=1M //生产一个1G大小的dd.img镜像
[ubuntu@book:~]$ mkfs.ext4 dd.img  //格式化为 ext4文件系统类型。
[ubuntu@book:~]$ sudo mount -t ext4 dd.img /mnt  //挂载到 /mnt目录下，之后进行操作。
[ubuntu@book:~]$ sudo umount /mnt  //操作完成后卸载该挂载的镜像。
```
