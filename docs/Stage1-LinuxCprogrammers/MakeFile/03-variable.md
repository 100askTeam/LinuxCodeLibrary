# 3.变量
## 3.1 变量的定义及取值
Makefile 也支持变量定义，变量的定义也让的我们的 Makefile 更加简化，可复用。
变量的定义：一般采用大写字母，赋值方式像 C 语言的赋值方式一样，如下：

```bash
DIR = ./100ask/
```

变量取值：使用括号将变量括起来再加美元符，如下：

```bash
FOO = $(DIR)
```

变量可让 Makefile 简化可复用，上面个的 Makefile 文件，内容如下：

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

我们可以将其优化，如下：

```makefile
#Makefile 变量定义
OBJ = main.o add.o sub.o
output: $(OBJ)
	gcc -o output $(OBJ)
main.o: main.c
	gcc -c main.c
add.o: add.c
	gcc -c add.c
sub.o: sub.c
	gcc -c sub.c
clean:
	rm $(OBJ) output
```

我们分析一下上面简化过的 Makefile，第一行是注释，Makefile 的注释采用‘#’，而且不支持像 C 语言中的多行注释。第二行我们定义了变量 OBJ，并赋值字符串”main.o，add.o，sub.o“。其中第三，四，十三行，使用这个变量。这样用到用一个字符串的地方直接调用这个变量，无需重复写一大段字符串。
Makefile 除了使用‘=’进行赋值，还有其他赋值方式，比如‘:=’和‘?=’，接下来我们来对比一下这几种的区别：

赋值符‘=’
我们使用一个例子来说明赋值符‘=’的用法。Makefile 内容如下：

```makefile
PARA = 100
CURPARA = $(PARA)
PARA = ask

print:
	@echo $(CURPARA)
```

分析代码：第一行定义变量 PARA，并赋值为“100”，第二行定义变量 CURPARA，并赋值引用变量 PARA，此时 CURPARA 的值和 PARA 的值是一样的，第三行，将变量 PARA 的变量修改为“ask”。第六行输出 CURPARA 的值，echo 前面增加@符号，代表只显示命令的结果，不显示命令本身。
通过命令“make print”执行 Makefile，如下：

```bash
$ make print
ask
```

从结果上看，变量 CURPARA 的值并不是“100”，而是 PARA 的最后一次赋值。使用赋值符“=”设置的变量，它的值在运行时才能确定，这称为“延时变量”。
其实可以理解为在 C 语言中，定义一个指针变量指向一个变量的地址。如下：

```c
01 int a = 10;
02 int *b = &a;
03 a=20;
```

代码分析：我们见上面的 Makefile 的第二行的“=”替换成“:=”，重新编译，如下：

```bash
$ make print
100
```

从结果上看，变量 CURPARA 的值为“100”。使用“:=”设置的变量被称为“即时变量”，在赋值时就确定了它的值。

赋值符‘?=’

我们用两个 Makefile 来说明赋值符“?=”的用法。如下：

```makefile
第一个 Makefile：
PARA = 100
PARA ?= ask
print:
	@echo $(PARA)
```

编译结果：

```bash
$ make print
100
```

```makefile
第二个 Makefile:
PARA ?= ask
print:
	@echo $(PARA)
```

编译结果：

```bash
$ make print
ask
```

上面的例子说明，使用“?=”给变量设置值时，如果这个变量之前没有被设置过，那么“?=”才会起效果；如果曾经设置过这个变量，那么“?=”不会起效果。赋值符‘+=’
Makefile 中的变量是字符串，有时候我们需要给前面已经定义好的变量添加一些字符串进去,此时就要使用到符号“+=”，比如如下：

```makefile
OBJ = main.o add.o
OBJ += sub.o
```

这样的结果是 OBJ 的值为：”main.o，add.o，sub.o“。说明“+=”用作与变量的追加。

## 3.2 系统自带变量

系统自定义了一些变量，通常都是大写，比如 CC，PWD，CLFAG 等等，有些有默认值，有些没有，比如以下几种，如下：
* CPPFLAGS：预处理器需要的选项，如：-l
* CFLAGS：编译的时候使用的参数，-Wall -g -c
* LDFLAGS：链接库使用的选项，-L -l
其中：默认值可以被修改，比如 CC 默认值是 cc，但可以修改为 gcc：CC=gcc使用的例子，如下：

```
OBJ = main.o add.o sub.o
output: $(OBJ)
	gcc -o output $(OBJ)
main.o: main.c
	gcc -c main.c
add.o: add.c
	gcc -c add.c
sub.o: sub.c
	gcc -c sub.c

clean:
	rm $(OBJ) output
```

使用系统自带变量，如下：

```
CC = gcc
OBJ = main.o add.o sub.o
output: $(OBJ)
	$(CC) -o output $(OBJ)
main.o: main.c
	$(CC) -c main.c
add.o: add.c
	$(CC) -c add.c
sub.o: sub.c
	$(CC) -c sub.c

clean:
	rm $(OBJ) output
```

在上面例子中，系统变量 CC 不改变默认值，也同样可以编译，修改的目的是为了明确使用 gcc 编译。

## 3.3 自动变量
Makefile 的语法提供一些自动变量，这些变量可以让我们更加快速地完成Makefile 的编写，其中自动变量只能在规则中的命令使用，常用的自动变量如
下：
* $@：规则中的目标
* $<：规则中的第一个依赖文件
* $^：规则中的所有依赖文件
我们上面的例子继续完善，修改为采用自动变量的格式，如下:

```makefile
CC = gcc
OBJ = main.o add.o sub.o
output: $(OBJ)
	$(CC) -o $@ $^
main.o: main.c
	$(CC) -c $<
add.o: add.c
	$(CC) -c $<
sub.o: sub.c
	$(CC) -c $<

clean:
	rm $(OBJ) output
```

其中：第 4 行$^表示变量 OBJ 的值，即 main.o add.o sub.o，第四，第六，第八行的$<分别表示 main.c add.c sub.c。$@表示 output。
