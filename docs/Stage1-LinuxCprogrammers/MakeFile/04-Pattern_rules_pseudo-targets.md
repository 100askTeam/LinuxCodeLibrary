# 4.模式规则与伪目标
## 4.1模式规则

模式规则实在目标及依赖中使用%来匹配对应的文件，我们依旧使用上面的例子，采用模式规则格式，如下：

```makeflie
CC = gcc
OBJ = main.o add.o sub.o
output: $(OBJ)
	$(CC) -o $@ $^
%.o: %.c
	$(CC) -c $<

clean:
	rm $(OBJ) output
```

其中：第五行%.o: %.表示如下。
1.main.o 由 main.c 生成
2.add.o 由 add.c 生成
3.sub.o 由 sub.c 生成

## 4.2伪目标

在前面的例子中，我们直接执行“make”命令，它的目的是去执行第 1 个规则，这跟执行“make output”的效果是一样的。在这里，“output”既是规则的目标，也是一个实际的文件。
而伪目标是什么呢？对于以前的例子，先执行 make 命令，然后再执行“make clean”命令，如下

```bash
$make
gcc -c main.c
gcc -c add.c
gcc -c sub.c
gcc -o output main.o add.o sub.o
$make clean
rm *.o output
```

一切正常！接着我们做个手脚，在 Makefile 目录下创建一个 clean 的文件，然后依旧执行 make 和 make clean，如下：

```bash
$touch clean
$make
gcc -c main.c
gcc -c add.c
gcc -c sub.c
gcc -o output main.o add.o sub.o
$make clean
make: 'clean' is up to date.
```

为什么“make clean”时命令没有被执行？因为已经有名为 clean 的文件了，并且它的依赖是空的，执行规则的条件没满足。
伪目标就是为了解决这个问题，我们在 clean 前面增加“.PHONY:clean”，如下：

```makefile
CC = gcc
OBJ = main.o add.o sub.o
output: $(OBJ)
	$(CC) -o $@ $^
%.o: %.c
	$(CC) -c $<

.PHONY:clean
clean:
	rm $(OBJ) output
```

运行结果：

```bash
$make
gcc -c main.c
gcc -c add.c
gcc -c sub.c
gcc -o output main.o add.o sub.o
$make clean
rm *.o output
```

当一个目标被声明为伪目标后，make 在执行规则时就会默认它满足执行条件。
这样就提高了 make 的执行效率，也不用担心由于目标和文件名重名了。
伪目标的两大好处：
1.避免只执行命令的目标和工作目录下的实际文件出现名字冲突。
2.提高执行 Makefile 时的效率

