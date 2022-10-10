# 文档编辑类命令

## cat 命令

> 在终端上显示文件内容

``` bash
[ubuntu@book:~]$ cat file	//查看file文档内容
[ubuntu@book:~]$ cat 11111 > file  //重定向11111 到file文档内，也可以理解将 1111 > 写入到 file里面。
[ubuntu@book:~]$ cat /proc/cpuinfo > cpuinfo.txt //将cpuinfo信息写入到 cpuinfo.txt文档内。
[ubuntu@book:~]$ cat -n  file	//显示file文本里面的行号。
[ubuntu@book:~]$ cat > helloword.txt <<EOF
>Hello
>word
>1111
>EOF
[ubuntu@book:~]$ cat helloword.txt  //常用于脚本处理等。
Hello
word
1111
```



## echo命令

> 在终端上输出字符串，或者提示shell的值

```bash
[ubuntu@book:~]$ echo "hello word!" //在终端上输出 helloword 字符串
[ubuntu@book:~]$ echo `date` //执行date命令，进行输出显示。
[ubuntu@book:~]$ echo -e "Hello\n Word!" //输出的字符串 Hell换行。
[ubuntu@book:~]$ echo -e "\033[31m Hello word \033[0m" //输出红色字体的Hello word.
```



## gedit 命令

> 一个文本编辑器，类似于VIM

```bash
[ubuntu@book:~]$  gedit file1  //gedit命令后面加 文件名，新文件或者已经存在的都可以。
```



## rm命令

> rm --help查看详细官方说明，主要用途是删除指定的文件或者目录。

```bash
[ubuntu@book:~]$ rm file    //删除一个已经存在的文件。
[ubuntu@book:~]$ rm -r dir  //删除一个目录。
[ubuntu@book:~]$ rm -v file  //显示删除文件国产的详细信息。
[ubuntu@book:~]$ rm -rv dir  //显示删除文件夹的详细输出信息。
[ubuntu@book:~]$ rm -d dir	  //删除空目录。
[ubuntu@book:~]$ rm -i file  //删除前确认，默认 y 表示确认，其它字符表示不确认。
```

## grep 命令

> grep --help可以查看官方详细说明。主要用途是文本字符串搜索。

```bash
[ubuntu@book:~]$ grep "fpu" cpuinfo.txt //在cpuinfo文件内搜索 fpu关键字。
[ubuntu@book:~]$ grep "fpu*" cpuinfo.txt //在cpuinfo文件内搜索 fpu*通配符关键字。
[ubuntu@book:~]$ grep -n "fpu" cpuinfo.txt //在cpuinfo搜索fpu关键字并显示关键所在行。
[ubuntu@book:~]$ grep -nr "fpu" DIR //在dir搜索所有文件包含fpu 关键字并显示关键并所在行以及文件名称。
[ubuntu@book:~]$ grep -nwr "fpu" DIR //在dir搜索所有文件包含fpu关键字以整词匹配方式，并显示并显示所在行以及文件名称。
[ubuntu@book:~]$ grep -wrh "fpu" DIR //在dir搜索所有文件包含fpu关键字以整词匹配方式，并显示并显示所在行不显示文件名称。
```

* 扩展：也可以使用egrep 命令进行搜索。



## more命令

> 以一页一页的形式在终端内显示文档内容。

```bash
[ubuntu@book:~]$ more file.txt   //查看file.txt内容，以页的方式查看。
```

* ctrl + F ：向下滚动一屏。 

* 空格键 ：向下滚动一屏。 

* ctrl + B : 向上滚动一屏。

* q : 退出。
