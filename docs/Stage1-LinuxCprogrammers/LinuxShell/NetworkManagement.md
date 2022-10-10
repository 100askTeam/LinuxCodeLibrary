# 网络管理

## ifconfig命令

> ifconfig用于管理网络设备。

```bash
[ubuntu@book:~]$ sudo apt install net-tools
[ubuntu@book:~]$ ifconfig
[ubuntu@book:~]$ sudo dhclient 			//自动获取IP地址
[ubuntu@book:~]$ sudo ifconfig ens33 up  //启动网卡名为ens33的网卡设备
[ubuntu@book:~]$ sudo dhclient 			//自动获取IP地址
[ubuntu@book:~]$ ping qq.com 			//测试网络联通性。

[ubuntu@book:~]$ sudo ifconfig ens33 down  //关闭网卡名为ens33的网卡设备
[ubuntu@book:~]$ sudo ifconfig ens33 hw ether 00:AA:BB:CC:DD:EE  //设置ens33网卡设备的MAC地址为00:AA:BB:CC:DD:EE。
[ubuntu@book:~]$ sudo ifconfig ens33 up  //启动网卡名为ens33的网卡设备
[ubuntu@book:~]$ sudo dhclient 			//自动获取IP地址
```

* 设置动态IP参考

```bash
book@virtual-machine:~$ cat /etc/netplan/01-netplan.yaml
network:
 version: 2
 ethernets:
    ens33:
      dhcp4: true
```



* 设置静态IP参考

```bash
book@virtual-machine:~$ cat /etc/netplan/01-netplan.yaml
network:
 version: 2
 renderer: networkd
 ethernets:
    ens192:
      dhcp4: no
      addresses: [192.168.1.133/24]
      gateway4: 192.168.1.1
```



## ping命令

> 用于检测或验证网络连通性。

```ba
[ubuntu@book:~]$ ping qq.com	//验证与qq.com的连通性
[ubuntu@book:~]$ ping 192.168.1.1 //验证与192.168.1.1的连通性
[ubuntu@book:~]$ ping -I ens36 qq.com //指定使用ens36网卡去验证与qq.com网站的连通性。
[ubuntu@book:~]$ ping -I ens36 192.168.1.1  //指定使用ens36网卡去验证与IP地址192.168.1.1的连通性。
[ubuntu@book:~]$ ping  -i 3 -s 1024 qq.com  //设置3秒 数据包大小1024 去验证和qq.com的连通性。
```

* 网络联通状态。

![image-20220424094726424](C:\Users\livel\Desktop\Linux常用命令.assets\image-20220424094726424.png)

* 网络通信异常情况。

![image-20220424094756125](C:\Users\livel\Desktop\Linux常用命令.assets\image-20220424094756125.png)





## samba共享文件

```bash
[ubuntu@book:~]$ sudo apt install cifs-utils smaba -y
[ubuntu@book:~]$ sudo nano /etc/samba/smb.conf
[share]
   comment = All Printers
   available = yes
   browseable = yes
   path = /home/ubuntu/Downloads
   public  = yes
   writable = yes
[ubuntu@book:~]$ sudo smbpasswd -a ubuntu
[ubuntu@book:~]$ sudo /etc/init.d/smbd restart 
[ubuntu@book:~]$ sudo chmod -R go+rwx  Downloads/
```

* Windows下输入`\\IP\share  `在弹出的对话框内输入我们刚才设置 smbpasswd 用户名和密码。

![image-20220424101726061](C:\Users\livel\Desktop\Linux常用命令.assets\image-20220424101726061.png)



## nfs服务

> NFS即网络文件系统，允许开发板直接通过网络挂载PC机中的文件夹。

```bash
[ubuntu@book:~]$ sudo apt install nfs-kernel-server
[ubuntu@book:~]$ mkdir ~/nfs_rootfs
[ubuntu@book:~]$ sudo nano /etc/exports
/home/ubuntu/nfs_rootfs  *(rw,nohide,insecure,no_subtree_check,async,no_root_squash)
[ubuntu@book:~]$ sudo service nfs-kernel-server restart

[ubuntu@book:~]$ sudo mount -t nfs 127.0.0.1:/home/ubuntu/nfs_rootfs /mnt
```



## tftp服务

```bash
[ubuntu@book:~]$  sudo apt-get install tftp-hpa tftpd-hpa
[ubuntu@book:~]$ mkdir ~/tftpboot
[ubuntu@book:~]$ sudo chmod -R 777 ~/tftpboot
[ubuntu@book:~/tftpboot]$ sudo nano /etc/default/tftpd-hpa
TFTP_USERNAME="tftp"
TFTP_DIRECTORY="/home/ubuntu/tftpboot"
TFTP_OPTIONS="-l -c -s"
TFTP_ADDRESS=":69"
[ubuntu@book:~/tftpboot]$ sudo service tftpd-hpa restart
```

> TFTP_OPTIONS="-l -c -s" -s表示安全 -c表示运行上传

* 开发板内使用tftp命令上传下载文件

```bash
# tftp -r test1 -g 192.168.1.20   //从192.168.1.20服务器获取到 test1文件。
# tftp -l 1.txt   -p 192.168.1.20  //将本地的1.txt上传到 192.168.1.20服务器。

```



## netstat命令

> 用于查看系统网络详细连接信息。

```bash
[ubuntu@book:~]$ netstat -a  //显示详细的网路状况。
[ubuntu@book:~]$ netstat -apu //显示端口号等使用情况。
```





## tcpdump命令

> 用于侦听网络传输的数据。

```bash
[ubuntu@book:~]$ tcpdump 	//显示所有的网络包信息。
[ubuntu@book:~]$ tcpdump -i ens36  //指定网卡设备，监听ens36的网络包信息
[ubuntu@book:~]$ tcpdump -i ens36 -q //以精简模式显示
[ubuntu@book:~]$ tcpdump -i ens36 -vv //显示更详细的信息。
[ubuntu@book:~]$ tcpdump -c 10 -i ens36 -vv //监听ens36,只抓取10个包。
[ubuntu@book:~]$ tcpdump -c 10 -i ens36 -vv  > tcpdump.cap //监听ens36,只抓取10个包,导出到tcpudump.cap文件内。
```



## ip命令

> 比ifconfig更强大的一个 网络管理命令。

```bash
[ubuntu@book:~]$ ip link show 
[ubuntu@book:~]$ ip a 
[ubuntu@book:~]$ ip addr show
[ubuntu@book:~]$ ip link set ens36 up
[ubuntu@book:~]$ ip link set ens36 down
[ubuntu@book:~]$ ip addr add 192.168.1.20/24 dev ens36 
[ubuntu@book:~]$ ip addr del 192.168.1.20/24 dev ens36 
[ubuntu@book:~]$ ip -s link list 
[ubuntu@book:~]$ ip route list
```