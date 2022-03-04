## 编程工具

### 编译链接

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

### Linux编译链接

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

#### make & makefile