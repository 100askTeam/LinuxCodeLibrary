* 问题反馈交流区 https://github.com/100askTeam/Stage1_D1s-Applications/discussions/
# 1.Makefile简述

## 1.1 为什么需要Makefile
现在一些项目工程中的源文件不计其数，其按类型、功能、模块分别放在若干个目录中，如果仍然在终端输入这些命令来编译，那显然不切实际，开发效率极低。
我们需要一个工具来管理这些编译过程，这就是“make”。make 是一个应用程序，它根据 Makefile 来做事。Makefile 负责管理整个编译流程：要编译哪些文件？怎么编译这些文件？怎么把它们链接成一个可执行程序。Makefile 定义了一系列的规则来实现这些管理。

## 1.2 makefie的引入
Makefile 的引入是为了简化我们编译流程，提高我们的开发进度。下面我们用一个例子来说明 Makefile 如何简化我们的编译流程。我们创建一个工程内容分别 main.c，sub.c，sub.h，add.c，add.h 五个文件。sub.c 负责计算两个数减法运算，add.c 负责计算两个数加法运算，然后编译出可执行文件。其源文件内容如下：

```c
main.c：

#include <stdio.h>
#include "add.h"
#include "sub.h"
int main()
{
	printf("100 ask, add:%d\n", add(10, 10));
	printf("100 ask, sub:%d\n", sub(20, 10));
	return 0;
}
```

```c
add.c：
#include "add.h"
int add(int a, int b)
{
	return a + b;
}
```

```c
add.h：
#ifndef __ADD_H
#define __ADD_H
int add(int a, int b);
#endif
```

```c
sub.c：
#include "sub.h"
int sub(int a, int b)
{
return a - b;
}
```

```c
sub.h：
#ifndef __SUB_H
#define __SUB_H
int sub(int a, int b);
#endif
```

我们使用 gcc 对上面工程进行编译及生成可执行程序，在终端输入如下命令，如下：

```bash
$ gcc main.c sub.c add.c -o ouput
$ ls
add.c add.h main.c output sub.c sub.h
$ ./output
100 ask, add:20
100 ask, sub:10
```

上面的命令是通过 gcc 编译器对 main.c sub.c add.c 这三个文件进行编译，及生成可执行程序 output，并执行可执行文件产生结果。
从上面的例子看起来比较简单，一条命令完成三个源程序的编译并产生结果，这是因为目前只有三个源文件。

如果有上千个源文件，即使你只修改了其中一个文件，你执行这样的命令时，它会把所有的源文件都编译一次。这样消耗的时间是非常恐怖的。
我们想实现：哪个文件被修改了，只编译这个被修改的文件即可，其它没有修改的文件就不需要再次重新编译了。可以使用下列命令，先单独编译文件，再最后链接，如下：

```bash
$ gcc -c main.c
$ gcc -c sub.c
$ gcc -c add.c
$ gcc main.o sub.o add.o -o output
```

我们将上面一条命令变成了四条，分别编译出源文件的目标文件，最后再将所有的目标文件链接成可执行文件。
当其中一个源文件的内容发生了变化，我们只需要单独编译它，然后跟其他文件重新链接成可知执行文件，不再需要重新编译其他文件。
假如我们修改了 add.c 文件，只需要重新编译生成 add.c 的目标文件，然后再将所有的.o 文件链接成可执行文件，如下：

```bash
$ gcc -c add.c
$ gcc main.o sub.o add.o -o output
```

这样的方式虽然可以节省时间，但是仍然存在几个问题，如下：

* 1. 如果源文件的数目很多，那么我们需要花费大量的时间，敲命令执行。
* 2. 如果源文件的数目很多，然后修改了很多文件，后面发现忘记修改了什么。
* 3. 如果头文件的内容修改，替换，更换目录，所有依赖于这个头文件的源文件全部需要重新编译。
这些问题我们不可能一个一个去找和排查，引入 Makefile 可以解决上述问题。我们先扔出一个 Makefile，如下：



```makefile
Makefile：
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

Makefile 编写好后只需要执行 make 命令，就可以自动帮助我们编译工程。注意，make 命令必须要在 Makefile 的当前目录执行，如下：

```bash
$ ls
add.c add.h main.c Makefile sub.c sub.h
$ make
gcc -c main.c
gcc -c add.c
gcc -c sub.c
gcc -o output main.o add.o sub.o
$ ls
add.c add.h add.o main.c main.o Makefile output sub.c sub.h sub.o
```

通过 make 命令就可以生成相对应的目标文件.o 和可执行文件。
如果我们再次使用 make 命令编译，如下，它说你的程序已经是最新的了，不需要再做什么事情：

```bash
$ make
make: 'output' is up to date.
```

我们可以修改一下 add.c，然后再执行 make，如下：

```bash
$ make
gcc -c add.c
gcc -o output main.o add.o sub.o
```

会发现，它重新编译了 add.c，并且重新生成了可执行程序。
通过上述例子，Makefile 将我们上面的三个问题都解决了，无需手工输入复杂的命令，只编译修改过的文件。在一个很庞大的工程中，只有第一次编译时间比较长，第二次编译时会大大缩短时间，节省了我们的开发周期。
下面我们来学习 Makefile 的知识。

