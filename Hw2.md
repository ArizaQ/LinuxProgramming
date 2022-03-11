1. 用命令行打印 HOME、PATH、SHLVL、LOGNAME 变量的值。

   ![image-20220311103641590](D:\PictureBed\image-20220311103641590.png)

2. 请用中文解释 Shell 脚本程序，并说明运行结果：

   解释：

   - clear：清屏

   - select

     bash的一种扩展应用，用于交互场合，表示用户可以从一组不同值中选择。

     只能用bash命令运行: `bash test.sh`，否则会报错。

     选项以数字编号，输入数字后自动将对应的选项加入变量item中。

   - "$item“ 表示item的变量值

   - case

     表示选择item的值的情况，如果是Continue就什么都不做，如果是Finish就退出。如果是其他情况(*)就打印一条提示信息。

   意义是有两个选项让用户循环选择，选择后分别执行三种操作: 什么都不做，退出，打印提示符。

   ![image-20220311105259206](D:\PictureBed\image-20220311105259206.png)

3. 阅读下列 Makefile 并用中文说明其含义。

   1. ```makefile
      # export，导出以下变量到子(下层makefile)。
      #Top导出为执行shell命令pwd后得到的当前工作目录
      # 假设shell 执行pwd返回值是/pwd/
      export Top:=${shell pwd}
      # 导出Src， Include， Build
      export Src:=$(Top)/src/
      export Include:=$(Top)/include/
      export Build:=$(Top)/build/
      # all是一个伪目标，同时是默认目标，执行make或make all都会执行它。
      # all的工作是：执行 make -C /pwd/src/
      all:
       @$(MAKE) -C $(Src)
       # 执行 cp /pwd/build/test /pwd/, 把/pwd/build/test移动到/pwd/下
      install:
       @cp $(Build)/test $(Top)
       # -rm -rf /pwd/build /pwd/test, 删除这两个目录下所有文件
      clean:
       @-rm -rf $(Build) $(Top)/test
      ```

   2. ```makefile
      # all 伪目标依赖于main.o test4.o,执行前先执行生成这两个的命令
      all:main.o test4.o
      # mkdir -p /pwd/build/, 新建目录，没有就新建。
       @mkdir -p $(Build)
       # 把所有的.o结尾的文件移动到/pwd/build/
       @mv *.o $(Build)
       # make -C /pwd/src/dir1
       $(MAKE) -C $(Src)/dir1
       # make -C /pwd/src/dir2
       $(MAKE) -C $(Src)/dir2
       # gcc -o /pwd/build/test /pwd/build/*.o /pwd/build/dir1/*.o
       # gcc 链接，目标存到/pwd/build/test，源文件是/pwd/build/和/pwd/build/dir1/ 下的所有.o文件
       $(CC) -o $(Build)/test $(Build)/*.o $(Build)/dir1/*.o $(Build)/dir2/*.o
      # 生成mian.o依赖于Include目录下的func.h
      # 执行 gcc -c main.c -I指定的目录，表示在该目录下搜索头文件并预处理，编译，汇编，生成main.o(默认的名字)
      main.o : $(Include)/func.h
       $(CC) -c main.c -I$(Include)
      ```