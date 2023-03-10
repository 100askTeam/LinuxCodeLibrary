# 5.Makefile函数
Makefile 提供了大量的函数，函数调用的格式如下：

```
$(function arguments)
```

这里`function’是函数名，`arguments’是该函数的参数。参数和函数名之间是用空格或 Tab 隔开，如果有多个参数，它们之间用逗号隔开。这些空格和逗号不是参数值的一部分。
我们经常使用的函数主要有两个(wildcard，patsubst)，先把它们单独拎出来讲讲。创建一个文件夹 src，在里下面创建两个文件，100.c，ask.c。如下：

```bash
.
├── Makefile
└── src
	├── 100.c
	└── ask.c
```

## 5.1常用函数

> **wildcard 函数**

用于查找指定目录下指定类型的文件，函数参数：目录+文件类型，如下：

```makefile
$(wildcard 指定文件类型)
```

其中，指定文件类型，如果不写路径，则默认为当前目录查找，例子如下：

```makefile
01 SRC = $(wildcard ./src/*.c)
02
03 print:
04 	@echo $(SRC)
```

执行命令 make，结果如下：

```bash
$ make
./src/ask.c ./src/100.c
```

其中，这条规则表示：找到目录./src 下所有后缀为.c 的文件，并赋值给变量SRC。命令执行完，SRC 变量的值：./src/ask.c ./src/100.c





> **patsubst 函数**

用于匹配替换。函数参数：原模式+目标模式+文件列表，如下:

```makefile
$( patsubst 原模式, 目标模式, 文件列表)
```

其中，从文件列表中查找出符合原模式文件类型的文件，然后一一替换成目标模式。举例：将./src 目录下的.c 结尾的文件，替换成.o 文件，并赋值给 obj。
如下：

```makefile
SRC = $(wildcard ./src/*.c)
OBJ = $(patsubst %.c, %.o, $(SRC))
print:
	@echo $(OBJ)
```

执行命令 make，结果如下：

```
$ make
./src/ask.o ./src/100.o
```

其中，这条规则表示：把变量中所有后缀为.c 的文件替换为.o。 命令执行完，OBJ 变量的值：./src/ask.o ./src/100.o



## 5.2 字符串替换和分析函数

> **$(subst from,to,text)**

在文本`text’中使用`to’替换每一处`from’。比如：

```makefile
$(subst ee,EE,feet on the street)
```

结果为‘fEEt on the strEEt’。



> **$(patsubst pattern,replacement,text)**

寻找`text’中符合格式`pattern’的字，用`replacement’替换它们。
`pattern’和`replacement’中可以使用通配符。比如：

```makefile
$(patsubst %.c,%.o,x.c.c bar.c）
```

结果为：`x.c.o bar.o’。

**$(strip string)**
去掉前导和结尾空格，并将中间的多个空格压缩为单个空格。比如：

```makefile
$(strip a b c )
```

结果为`a b c’。



> **$(findstring find,in)**

在字符串`in’中搜寻`find’，如果找到，则返回值是`find’，否则返回值为空。比如：

```makefile
$(findstring a,a b c)
$(findstring a,b c)
```

将分别产生值`a’和`’(空字符串)。



> **$(filter pattern...,text)**

返回在`text’中由空格隔开且匹配格式`pattern...’的字，去除不符合格式`pattern...’的字。比如

```makefile
$(filter %.c %.s,foo.c bar.c baz.s ugh.h)
```

结果为`foo.c bar.c baz.s’。



> **$(filter-out pattern...,text)**

返回在`text’中由空格隔开且不匹配格式`pattern...’的字，去除符合格式`pattern...’的字。它是函数 filter 的反函数。比如：

```makefile
$(filter %.c %.s,foo.c bar.c baz.s ugh.h)
```

结果为`ugh.h’。



> **$(sort list)**

将‘list’中的字按字母顺序排序，并去掉重复的字。输出由单个空格隔开的字的列表。比如：

```makefile
$(sort foo bar lose)
```

返回值是‘bar foo lose’。


***

## 5.3 文件名函数

> **$(dir names...)**

抽取‘names...’中每一个文件名的路径部分，文件名的路径部分包括从文件名的首字符到最后一个斜杠(含斜杠)之前的一切字符。比如：

```makefile
$(dir src/foo.c hacks)
```

结果为‘src/ ./’。



> **$(notdir names...)**

抽取‘names...’中每一个文件名中除路径部分外一切字符（真正的文件名）。比如：

```makefile
$(notdir src/foo.c hacks)
```

结果为‘foo.c hacks’。



> **$(suffix names...)**

抽取‘names...’中每一个文件名的后缀。比如：

```makefile
$(suffix src/foo.c src-1.0/bar.c hacks)
```

结果为‘.c .c’。



> **$(basename names...)**

抽取‘names...’中每一个文件名中除后缀外一切字符。比如：

```makefile
$(basename src/foo.c src-1.0/bar hacks)
```

结果为‘src/foo src-1.0/bar hacks’。



> **$(addsuffix suffix,names...)**

参数‘names...’是一系列的文件名，文件名之间用空格隔开；suffix 是一个后缀名。将 suffix(后缀)的值附加在每一个独立文件名的后面，完成后将文件名串联起来，它们之间用单个空格隔开。比如：

```makefile
$(addsuffix .c,foo bar)
```

结果为‘foo.c bar.c’。



> **$(addprefix prefix,names...)**

参数‘names’是一系列的文件名，文件名之间用空格隔开；prefix 是一个前缀名。将 preffix(前缀)的值附加在每一个独立文件名的前面，完成后将文件名串联起来，它们之间用单个空格隔开。比如：

```makefile
$(addprefix src/,foo bar)
```

结果为‘src/foo src/bar’。



> **$(wildcard pattern)** 

参数‘pattern’是一个文件名格式，包含有通配符(通配符和 shell 中的用法一样)。函数 wildcard 的结果是一列和格式匹配的且真实存在的文件的名称，文件名之间用一个空格隔开。
比如若当前目录下有文件 1.c、2.c、1.h、2.h，则:

```makefile
c_src := $(wildcard *.c)
```

结果为‘1.c 2.c’。



## 5.4 其他函数

> **$(foreach var,list,text)**

前两个参数，‘var’和‘list’将首先扩展，注意最后一个参数‘text’，此时不扩展；接着，‘list’扩展所得的每个字，都赋给‘var’变量；然后‘text’引用该变量进行扩展，因此‘text’每次扩展都不相同。
函数的结果是由空格隔开的‘text’ 在‘list’中多次扩展后，得到的新‘list’，就是说：‘text’多次扩展的字串联起来，字与字之间由空格隔开，如此就产生了函数 foreach 的返回值。
下面是一个简单的例子，将变量‘files’的值设置为 ‘dirs’中的所有目录下的所有文件的列表：

```makefile
dirs := a b c d
files := $(foreach dir,$(dirs),$(wildcard $(dir)/*))
```

这里‘text’是‘$(wildcard $(dir)/*)’，它的扩展过程如下：

a) 第一个赋给变量 dir 的值是`a’，扩展结果为‘$(wildcard a/*)’；
b) 第二个赋给变量 dir 的值是`b’，扩展结果为‘$(wildcard b/*)’；
c) 第三个赋给变量 dir 的值是`c’，扩展结果为‘$(wildcard c/*)’；
d) 如此继续扩展。

这个例子和下面的例有共同的结果：

```makefile
files := $(wildcard a/* b/* c/* d/*)
```

> **$(if condition,then-part[,else-part])**

首先把第一个参数‘condition’的前导空格、结尾空格去掉，然后扩展。如果扩展为非空字符串，则条件‘condition’为‘真’；如果扩展为空字符串，则条件‘condition’为‘假’。
如果条件‘condition’为‘真’,那么计算第二个参数‘then-part’的值，并将该值作为整个函数 if 的值。
如果条件‘condition’为‘假’,并且第三个参数存在，则计算第三个参数‘else-part’的值，并将该值作为整个函数 if 的值；如果第三个参数不存在，函数 if 将什么也不计算，返回空值。
注意：仅能计算‘then-part’和‘else-part’二者之一，不能同时计算。这样有可能产生副作用（例如函数 shell 的调用）。

> **$(origin variable)**

变量‘variable’是一个查询变量的名称，不是对该变量的引用。所以，不能采用‘$’和圆括号的格式书写该变量，当然，如果需要使用非常量的文件名，可以在文件名中使用变量引用。

函数 origin 的结果是一个字符串，该字符串变量是这样定义的：

* ‘undefined'：如果变量‘variable’从没有定义；
*  ‘default'：变量‘variable’是缺省定义；
*  ‘environment'：变量‘variable’作为环境变量定义，选项‘-e’没有打开；
*  ‘environment override'：变量‘variable’作为环境变量定义，选项‘-e’已打开；
*  ‘file' ：变量‘variable’在 Makefile 中定义；
*  ‘command line'：变量‘variable’在命令行中定义；
*  ‘override'：变量‘variable’在 Makefile 中用 override 指令定义；
*  ‘automatic' ：变量‘variable’是自动变量


> **$(shell command arguments)**

函数 shell 是 make 与外部环境的通讯工具。函数 shell 的执行结果和在控制台上执 行‘ command arguments ’的 结果相似。 不过如果 ‘ command arguments’的结果含有换行符（和回车符），则在函数 shell 的返回结果中将把它们处理为单个空格，若返回结果最后是换行符（和回车符）则被去掉。
比如当前目录下有文件 1.c、2.c、1.h、2.h，则：

```makefile
c_src := $(shell ls *.c)
```

结果为‘1.c 2.c’。



