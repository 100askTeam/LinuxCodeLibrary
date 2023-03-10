# 2. makefile规则
## 2.1 命名规则
一般来说将 Makefile 文件取名为“Makefile”或“makefile”都可以，惯例是使用首字母大写的“Makefile”。也可以使用其他名字，比如makefile.linux，但你需要用“-f”参数指定，示例如下

```bash
make -f makefile.linux
```

## 2.2 语法规则
基本语法规则：

![image](https://github.com/1WMD1/script_file/blob/master/makefile/png/image-20230310110255519.png)

* target：需要生成的目标文件
* prerequisites：生成该 target 所依赖的一些文件
* command：生成该目标需要执行的命令



三者的关系：target 依赖于 prerequisites 中的文件，其生成规则定义在command 中。举例，比如我们平时要编译一个文件：

```bash
$ gcc main.c -o main
```

换成 Makefile 的书写格式如下：

```makefile
main:main.c
	gcc main.c -o main
```

第 1 行表示 main 这个可执行程序依赖于 main.c，第 2 行表示需要用“gcc main.c -o main”这个命令来生成 main。
注意：Makefile 文件里的命令必须要使用 Tab，不能使用空格。

## 2.3 目标生成规则

* 目标生成：

> 检查规则中的依赖文件是否存在。

> 若依赖文件不存在，则寻找是否有规则用来生成该依赖文件。

目标生成流程，如下：

![image](https://github.com/1WMD1/script_file/blob/master/makefile/png/image-20230310111353906.png)

* 目标更新：

> 检查目标的所有依赖，任何一个依赖有更新时，就要重新生成目标。
> 目标文件比依赖文件更新时间晚，则需要更新。

目标更新流程，如下：

![image](https://github.com/1WMD1/script_file/blob/master/makefile/png/image-20230310111817321.png)

我们使用上面的例子，Makefile 内容如下：

```makefile
output: main.o add.o sub.o
	gcc -o output main.o add.o sub.o
main.o: main.c
	gcc -c main.c
add.o: add.c
	gcc -c add.c
sub.o: sub.c
	gcc -c sub.c
clean:
	rm *.o output
```

编译执行：

```bash
$ make
gcc -c main.c
gcc -c add.c
gcc -c sub.c
gcc -o output main.o add.o sub.o
```

make 命令会检测寻找目标的依赖是否存在，不存在，则会寻找生成依赖的命令。
1.output 依赖于“main.o add.o sub.o”，这三个文件都没有，分别去生成它们。怎么生成？继续寻找规则，发现 main.o 依赖于 main.c，于是使用“gcc -c main.c”来生成；其他两个文件类似处理。

2.现在 output 的依赖文件都有了，但是 output 文件还没有，所以使用“gcc -o output main.o add.o sub.o”来生成 output 文件。当我们修改某一个文件时，比如之修改 add.c 文件，然后重新 make，如下：

```bash
$ make
gcc -c add.c
gcc -o output main.o add.o sub.o
```

make 命令还是根据 Makefile 来检查、执行：
1.output 依赖于“main.o add.o sub.o”，这三个文件都有了，main.o 依赖于 main.c，main.c 没改过，所以不需要重新生成 main.o；
但是 add.o的依赖文件 add.c 被改过，需要重新生成 add.o，使用命令“gcc -c add.c”；sub.o 跟 main.o 类似，也不需要重新生成。
2.现在 output、它的三个依赖文件都有了，但是其中一个依赖文件 add.o 比output 文件新，所以使用“gcc -o output main.o add.o sub.o”来生成output 文件。

