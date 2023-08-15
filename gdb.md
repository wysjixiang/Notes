### GDB使用 https://zhuanlan.zhihu.com/p/357360607





1. 首先需要在编译的时候添加 -g选项编译源文件才可以生成满足GDB要求的可执行文件
2. 各种调试命令

![image-20230814124556502](/home/jixiang/.config/Typora/typora-user-images/image-20230814124556502.png)

3. run和start命令都会让gdb开始执行，不同的是run会一直运行，直到断点或者结束；而start会在main函数处暂停

4. break的语法格式：

   1. b + 位置，如函数，或者文件中的第几行
   2. b + if condition

5. `clear` 命令可以删除指定位置处的所有断点，常用的语法格式如下所示：

   ```text
   (gdb) clear location
   ```

​	参数`location` 通常为某一行代码的行号或者某个具体的函数名。当 `location` 参数为某个函数的函数名时，表示删除位于该函数入口处的所有断点。

`delete` 命令（可以缩写为 `d`）通常用来删除所有断点，也可以删除指定编号的各类型断点，语法格式如下：

```text
delete [breakpoints] [num]
```

其中，`breakpoints` 参数可有可无，`num` 参数为指定断点的编号，其可以是`delete` 删除某一个断点，而非全部。

如果不指定 `num`参数，则 `delete` 命令会删除当前程序中存在的所有断点。



6. 

   ### **查看堆栈信息**

   ### **backtrace 命令**

   `backtrace` 命令用于打印当前调试环境中所有栈帧的信息，常用的语法格式如下：

   ```text
   (gdb) backtrace [-full] [n]
   ```

   其中，用 [ ] 括起来的参数为可选项，它们的含义分别为：

   - `n`：一个整数值，当为正整数时，表示打印最里层的 `n` 个栈帧的信息；`n`为负整数时，那么表示打印最外层`n`个栈帧的信息；
   - `-full`：打印栈帧信息的同时，打印出局部变量的值。

   注意，当调试多线程程序时，该命令仅用于打印当前线程中所有栈帧的信息。如果想要打印所有线程的栈帧信息，应执行`thread apply all backtrace`命令。