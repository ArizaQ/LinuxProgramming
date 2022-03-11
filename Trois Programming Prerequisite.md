[toc]

# 编程工具: 编译

## 编译链接

编译器一次只能处理一个.c文件

过程：

- 预处理

  #include 任何文本文件

  .c/.cpp    .h/.hh/.H => .i/.ii

- 编译 

  .i => .s/.S

- 汇编 

  .s =>.o

- 链接

  报错不会告知具体代码位置

  原因：项目太大
  
  .o .a => 可执行文件

库：

- 静态库 .a 或.lib

- 动态库 .so 或.dll

  优点：

  - 体积小
  - 修改后无需重新编译

Java

- 没有链接：每个class文件相当于一个动态库

.Net

- 类似Java。运行在.Net Framework
- xx.net: xx语言编译器. 产物叫 "中间语言"

Delphi

- 本地的

## Linux编译链接

#### gcc

```bash
gcc [options] [filename]

options:
	-E
	-S
	-c
	-o output_file
	-g # 添加调试信息：映射 目标代码行 => 源代码行(有路径信息)
	-On #n: 0~3
	-Wall
	-Idir
	-Ldir
	-lname #
	-DMACRO [=DEFN] # 让compiler增加#define宏
```

#### GDB

```bash
file
break/tbreak
run
list

next
step

display
print

kill
quit

shell
make
```

## make & makefile

- 多文件项目：
  - IDE
  - makefile

makefile

- 基于规则间的依赖。定义整个工程的编译规则
- 在链接时决定需要重新编译那些文件
- 自动化编译

make格式：make [-f Makefile] [option] [target]

- 默认名为Makefile

  可以自己制定.mk

- target:

  - 显式调用的目标(phony target)
  - 文件名

### Makefile规则结构

### target

```makefile
target: prerequisites...
	command
	...
```

伪目标：

- 无法生成他的依赖关系和决定是否执行。只能显式指明。
- 放在第一个就是默认目标

多目标：

- $@

  ```makefile
  bigoutput littleoutput : text.g
  	generate text.g -$(subst output,,$@) > $@
  ```

  函数subst: substitude 替换函数

### 预定义变量

- $(xx) 取变量值
- \ 换行
- $< 第一个依赖文件
- $? 所有晚的依赖文件
- $+ 所有依赖文件
- $^ 所有无重复的依赖文件
- $* 无扩展名的目标文件名
- $@ 当前目标文件
- $% 目标是归档成员，表示目标的归档成员名称(缩写)

### 多目标扩展

```makefile
<targets...>: <target-pattern>: <prereq-patterns...>
	commands
	...
```

### 函数

语法：

- `$(<function> <arguments>)`
- `${<function> <arguments>}`

常用函数

- $(subst
- `$(dir <names>)`
- `$(basename <names>)` 去扩展名
- `$(foreach <var>,<list>,<text>)`
- `$(if <condition>, <them-part>)`
- `$(if <condition>, <them-part>,<else-part>)`
- `$(call <expression>,<param1>,...)`

# Linux系统编程：文件系统

## 文件系统

文件系统：

- 内核中用来管理文件系统以及对文件进行操作的机制和实现。

文件类型

- regular
- 字符设备
- 块设备
- fifo管道
- socket
- 符号链接
  - 硬连接：不能跨分区
- 目录

文件结构：

- 字节流

符号连接 

### VFS

#### VFS 模型

- 组成

  - 超级块

  - i-node对象

    一个inode节点对应于磁盘上的一个文件(不是文件对象!!!)

  - 文件对象

    代表文件的打开(一次文件的访问)

  - 目录项对象

    记录磁盘上文件的逻辑结构

### Ext2 文件系统

组成

- 引导块

  1kb。存储分区信息和启动信息

- 块组0

  - 超级快

    整个分区的文件系统信息

  - 组描述符

  - Block bitmap

  - inode bitmap

  - Inode 表

    - inode

      ![这里写图片描述](D:\PictureBed\20180828095754703)

  - 数据块

- 块组1

- ...

### 软连接和硬连接

#### Hard link

`ln`

#### Symbolic link

`ln -s`

### 系统调用

- C函数形式
- 系统调用
- 库函数

## I/O

- 系统调用大部分非缓存，以文件描述符形式
- c库一般是带缓存的

### 文件描述符

- int fd: 非负整数值

  ```c
  #include <fcntl.h>
  int fd= open("data",O_READ_ONLY);
  int nread=read(fd,buf,1024);
  close(fd);
  ```

- 位于`<unistd.h>`

  已占用：0 stdin,1 stdout ,2 stderr

- 头文件

  ```c
  #include<sys/types.h>
  #include<sys/stat.h>
  #include<fcntl.h>
  ```

- open

  ```c
  create: 对open的调用，只是固定了flags= O_CREAT|O_WRONLY|O_TRUNC
  ```

  两个open的实现机制

  - c语言有可变参数

  flags

  - O_RDONLY
  - O_WRONLY
  - O_RDWR
  - O_APPEND
  - O_TRUNC 写文件时先丢掉原来的
  - O_CREAT
  - O_EXCL：文件存在时返回错误

  mode

  - ![image-20220311115102547](D:\PictureBed\image-20220311115102547.png)

  - umask

    