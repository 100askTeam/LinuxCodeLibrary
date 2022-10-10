# 文件管理类命令
## ls命令 
> 可以使用ls --help去看官方的说明。用于查看当前目录下的 目录 文件详细参数等信息。

``` bash
ls 
```
查看当然目前目录下的所有文件，以及文件夹，（不包含隐藏文件）。
``` bash
ls -l
```
查看当然目前目录下的所有文件，以及文件夹，的详细属性信息。（不包含隐藏文件）。
``` bash
ls -a
```
查看当然目前目录下的所有文件，以及文件夹，（包含隐藏文件）。
``` bash
ls DIR
```
查看DIR(目标目录)下的所有文件，（不包含隐藏文件）。 后面加同等的 ls 参数，比如 `ls Desktop -la` 就是查看 Desktop目录下的所有文件夹 文件 以及所有的隐藏文件还有详细的属性信息等。

## cd 命令： 
> 可以使用 cd --help去查看官方说明。 改变shell工作目录。

``` bash
cd DIR
```

进入（切换）到DIR目标目录下。比如使用 `cd Desktop` 进入到Desktop目录下。

``` bash
cd -
```

切换到上次执行cd DIR目录内。 举例，首先在 `~`  目录，之后我们使用`cd Desktop`进入到Desktop目录下，然后使用`cd -`命令直接切换回 `~`加目录下。

``` bash
cd .        //表示进入到当前目录
cd ..       //表示切换到上一级目录
cd  ../..   //表示切换到 上上级目录。
```



## cp命令：

>  可以使用 cp --help去查看官方说明。将指定的<源文件>复制至<目标文件>，或将多个<源文件>复制至<目标目录>。

``` bash
cp -r DIR1 DIR1   //复制DIR1目录到DIR2目录
cp file1 file2	  //复制file1文件到file2
cp -d link DIR    //复制一个链接文件到DIR目录内
cp -p file1 file3 //复制file1文件及所有属性到file3
cp -f 				//强制复制 某个文件或者文件夹
cp -v 				//显示复制的详细信息等。
```



## mv命令

> 可以使用mv --help命令查看官方说明，将<源文件>重命名为<目标文件>，或将源文件移动至指定<目录>。

``` bash
mv file1 file2  //将file1重命名为file2
mv dir1 dir2	//将dir1重命名为dir2
mv file1 dir2	//将file1移动至dir2目录内。
mv dir2 dir1/	//将dir2目录移动至dir1目录内。
mv -f 			//强行移动或重命名操作。
mv -v 			//显示移动或者重命名等操作的详细信息。
```



## pwd命令

打印当前工作目录的完整路径。

``` bash
pwd				//显示完整的目录路径。
```



## mkdir命令

创建一个或多个不存在的 目录，或者在某个目录下创建目录。

``` bash
mkdir dir1  	//创建一个dir1目录
mkdir dir1 dir2 //创建一个dir1 dir2目录
mkdir -p dir1/dir1.1  //在dir1目录下创建一个 dir1.1目录
```

rmdir 删除一个dir目录或者多个dir目录，前提要求 dir目录下没有文件。

## chown

> 用于设置文件所有者和文件关联组的命令。

```bash
[ubuntu@book:~]$  chown root:root  test.txt  //修改这个文件的用户组和用户ID。
[ubuntu@book:~]$  chown  -R root:root ./*  //修改这个目录下所有文件及文件夹的用户组和用户ID。
```



## touch

> 用于修改文件或者目录的时间属性，或者创建一个新的文件。

```bash
[ubuntu@book:~]$ touch file1.txt  //修改这个文件的时间，如果文件不存在则创建一个文件。
```



## file

> 用于显示文件类型等详细信息。

```bash
[ubuntu@book:~]$ file file.txt  	//用于显示这个文件的详细类型信息。
[ubuntu@book:~]$ file -i file.txt  	//用于显示这个文件的MIME类别。
```
##　chmod命令

![img](C:\Users\livel\Desktop\Linux常用命令.assets\rwx-standard-unix-permission-bits.png)



```bash
[ubuntu@book:~]$  chmod ugo+x test.sh  //给这个文件增加所有用户的可执行权限。
[ubuntu@book:~]$  chmod a+x test.sh  //给这个文件增加所有用户的可执行权限。
[ubuntu@book:~]$  chmod a-x test.sh  //给这个文件移除所有用户的可执行权限。
[ubuntu@book:~]$  chmod 755 test.sh   //给这个文件user设置为rwx group设置为r x,给其它用户也是设置为r x.
[ubuntu@book:~]$  chmod -R 755 ./*  //给这个路径下的所有文件机及目录设置为 rwx rx rx权限。
```



## ln命令

> 创建链接文件命令

```bash
[ubuntu@book:~]$  ln -s file1.txt  doc.txt  //给file1.txt创建一个软链接名为doc.txt
[ubuntu@book:~]$  ln file1.txt  doc.txt  //给file1.txt创建一个硬链接名为doc.txt
```



## tar命令

> 对文件进行压缩与解压缩操作。

```bash
[ubuntu@book:~]$ tar -czf file.tar.gz file/   	//压缩 file/文件夹为gz格式，压缩包名称为 file.tar.gz
[ubuntu@book:~]$ tar -xzf file.tar.gz 	 		//解压缩 file.tar.gz压缩包
[ubuntu@book:~]$ tar -cjf file.tar.bz  file/ 	//压缩 file/文件夹为bz格式，压缩包名称为 file.tar.bz
[ubuntu@book:~]$ tar -xjf file.tar.gz 			//解压缩file.tar.gz压缩包.
[ubuntu@book:~]$ tar czf - file/ | split -d -b100m - file.tar.gz //将file文件夹进行分割，以100M大小进行分割操作，生产的压缩包格式为 file.tar.gz
[ubuntu@book:~]$ cat file.tar.gz0* | tar zx   //将分卷压缩的压缩包进行统一解压缩操作。
```



## 7z命令

> 以.7z结尾的压缩包进行解压缩或者压缩操作。

```bash
[ubuntu@book:~]$ sudo apt install p7zip-full  //安装7z压缩解压缩工具
[ubuntu@book:~]$ 7z a -t7z -r test1.7z  dir1/  //压缩dir1目录为test1.7z压缩包。
[ubuntu@book:~]$ 7z x  test1.7z -r -odir2/		//解压缩名为 test1.7z，递归解压输出到dir2目录内。
```



## zip命令

> 以.zip结尾的压缩包，压缩与解压缩。

```bash
[ubuntu@book:~]$ zip -r test1.zip dir1 file1 	//将dir1 file1压缩为 test1.zip压缩包。
[ubuntu@book:~]$ unzip -v test1.zip 	//查看压缩包test1.zip里面的内容
[ubuntu@book:~]$ unzip -t test1.zip		//查看一下test1.zip压缩包完整性
[ubuntu@book:~]$ unzip -d tmp  test1.zip //解压缩test1.zip到 tmp目录下。
[ubuntu@book:~]$ zip  test1.zip  -d file1	//单独从test1.zip压缩包里删除 file1文件。
[ubuntu@book:~]$ unzip -v test1.zip //查看压缩包test1.zip里面的内容
```



## gzip命令

> 压缩.gz结尾的压缩包。他只能压缩单个文件。

```bash
[ubuntu@book:~]$ gzip test1.iso  //压缩一个名为test1.iso为gz压缩格式。
[ubuntu@book:~]$ gunzip test1.iso.gz //解压缩test1.iso.gz压缩包。
```



## bzip2命令

> 压缩为.bz2的压缩包以及解压缩。

```bash
[ubuntu@book:~]$ bzip2 file.txt  		//压缩一个file.txt,压缩后名称为file.txt.bz2 
[ubuntu@book:~]$ bunzip2 file.txt.bz2   //解压缩file.txt.bz2 
```
