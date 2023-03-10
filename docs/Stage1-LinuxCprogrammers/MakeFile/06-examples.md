# 6.Makefile实例
在上面的例子中，我们都是把头文件，源文件放在同一个文件里面，这样不好用于维护，所以我们将其分类，把它变得更加规范一下，把所有的头文件放在文件夹：inc，把所有的源文件放在文件夹：src。
代码目录如下：

```bash
$ tree
.
├── inc
│ ├── add.h
│ └── sub.h
├── Makefile
└── src
	├── add.c
	├── main.c
	└── sub.c
```

其中 Makefile 的内容如下：

```makefile
SOURCE = $(wildcard ./src/*.c)
OBJECT = $(patsubst %.c, %.o, $(SOURCE))
INCLUEDS = -I ./inc

TARGET = 100ask
CC = gcc
CFLAGS = -Wall -g
$(TARGET): $(OBJECT)
	@mkdir -p output/
	$(CC) $^ $(CFLAGES) -o output/$(TARGET)

%.o: %.c
	$(CC) $(INCLUEDS) $(CFLAGES) -c $< -o $@

.PHONY:clean
clean:
	@rm -rf $(OBJECT) output/
```

分析：

* 行 1：获取当前目录下 src 所有.c 文件，并赋值给变量 SOURCE。
* 行 2：将./src 目录下的.c 结尾的文件，替换成.o 文件，并赋值给变量OBJECT。
* 行 4：通过-I 选项指明头文件的目录，并赋值给变量 INCLUDES。
* 行 6：最终目标文件的名字 100ask，赋值给 TARGET。
* 行 7：替换 CC 的默认之 cc，改为 gcc。
* 行 8：将显示所有的警告信息选项和 gdb 调试选项赋值给变量 CFLAGS。
* 行 11：创建目录 output，并且不再终端现实该条命令。
* 行 12：编译生成可执行程序 100ask，并将可执行程序生成到 output 目录
* 行 15：将源文件生成对应的目标文件。
* 行 17：伪目标，避免当前目录有同名的 clean 文件。
* 行 19：用与执行命令 make clean 时执行的命令，删除编译过程生成的文件。
  最后编译的结果，如下:

```bash
$ make
gcc -I ./inc -c src/main.c -o src/main.o
gcc -I ./inc -c src/add.c -o src/add.o
gcc -I ./inc -c src/sub.c -o src/sub.o
gcc src/main.o src/add.o src/sub.o -o output/100ask
$tree
.
├── inc
│ ├── add.h
│ └── sub.h
├── Makefile
├── output
│ └── 100ask
└── src
	├── add.c
	├── add.o
	├── main.c
	├── main.o
	├── sub.c
	└── sub.o
```

上面的 Makefile 文件算是比较完善了，不过项目开发中，代码需要不断的迭代，那么必须要有东西来记录它的变化，所以还需要对最终的可执行文件添加版本号，如下：

```makefile
VERSION = 1.0.0
SOURCE = $(wildcard ./src/*.c)
OBJECT = $(patsubst %.c, %.o, $(SOURCE))

INCLUEDS = -I ./inc

TARGET = 100ask
CC = gcc
CFLAGS = -Wall -g

$(TARGET): $(OBJECT)
	@mkdir -p output/
	$(CC) $^ $(CFLAGES) -o output/$(TARGET)_$(VERSION)

%.o: %.c
	$(CC) $(INCLUEDS) $(CFLAGES) -c $< -o $@

.PHONY:clean
clean:
	@rm -rf $(OBJECT) output/
```

* 行 1：将版本号赋值给变量 VERSION。
* 行 13：生成可执行文件的后缀添加版本号。

编译结果：

```
$ tree
.
├── inc
│ 	├── add.h
│ 	└── sub.h
├── Makefile
├── output
│ 	└── 100ask_1.0.0
└── src
	├── add.c
		├── add.o
	├── main.c
	├── main.o
	├── sub.c
	└── sub.o
```

