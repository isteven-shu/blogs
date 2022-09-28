# 启动GDB
The most usual way to start gdb is with one argument, specifying an **executable** program:
### 调试未启动程序
```
gdb program
```
You can also start with both an executable program and a core file specified:
```
gdb program core
```
You can optionally have gdb pass any arguments after the executable file to the inferior using `--args`. This option stops option processing.
```
gdb --args gcc -O2 -c foo.c
```
This will cause gdb to debug gcc, and to set gcc’s command-line arguments to `-O2 -c foo.c`.
### 调试已启动进程
You can, instead, specify a process ID as a second argument or use option `-p`, if you want to debug a running process:
```
gdb program 1234
```
would attach gdb to process 1234. 
```
gdb -p 1234
```
With option `-p` you can omit the program filename.
# 设置参数
```
set args
```
Specify the arguments to be used the next time your program is `run`. If set args has no arguments, `run` executes your program with no arguments.
# 运行程序
```
run
r
```
`run` with no arguments uses the same arguments used by the previous run, or those set by the set args command.
```
start
```
Setting a temporary breakpoint at the beginning of the main procedure and then invoking the `run` command.
```
starti
```
Setting a temporary breakpoint at the first instruction of a program’s execution and then invoking the `run` command.
# 在GDB内执行shell command
If you need to execute occasional shell commands during your debugging session, there is **no need to leave or suspend gdb**; you can just use the shell command.
```
shell command-string
!command-string
```
This will invoke a standard shell to execute command-string. Note that **no space** is needed between ! and command-string. If it exists, the environment variable SHELL determines which shell to run. Otherwise gdb uses the default shell (‘/bin/sh’ on Unix systems)
# Stack
### 列出所有stack frame
A backtrace is a summary of how your program got where it is. It shows one line per frame, for many frames, starting with the currently executing frame (frame zero), followed by its caller (frame one), and on up the stack. To print a backtrace of the entire stack, use the `backtrace` command, or its alias `bt`:
```
backtrace
bt
```
### 切换frame
```
frame n
f n
```
Recall that frame zero is the innermost (currently executing) frame, frame one is the frame that called the innermost one, and so on. The highest level frame is usually the one for main.
```
up n
down n
```
Move n frames up/down the stack. n defaults to 1.
# Breakpoint
### 设置断点
#### 设置普通断点
```
break location
b location
```
Set a breakpoint at the given location, which can specify a function name, a line number, or an address of an instruction.
When called without any arguments, `break` sets a breakpoint at the next instruction to be executed in the selected stack frame.
#### 设置条件断点
```
break ... if cond
```

#### 设置一次性断点
```
tbreak args
```
### 为已存在的断点设置/改变触发条件
```
condition bnum expression
cond bnum expression
```
You can specify a condition on an existing breakpoint by using the breakpoint number as a reference:
`(gdb) cond 3 i == 99`
And remove a condition from a breakpoint using:
`(gdb) cond 3`
Breakpoint 3 now unconditional.
### 列出所有断点
```
info b
```
# 打印数据
### 打印
- 打印普通变量、数组、字符串：直接`p`变量名即可
- 打印**指针**指向的内容，需要解引用
   - 仅仅使用`*`只能打印第一个值
   - 如果要打印多个值，后面跟上`@`并加上要打印的长度
     ```
      p *pointer@10
     ```

### 批量打印
打印所有全局/局部变量
- Type `info variables` to list "All global and static variable names" (huge list.
- Type `info locals` to list "Local variables of current stack frame" (names and values), including static variables in that function.
- Type `info args` to list "Arguments of the current stack frame" (names and values).
### 检查内存
```
x/nfu addr
```
- n: 重复次数，默认是1
- f：打印格式
   - x: 十六进制
   - d: 十进制
   - o：八进制
   - f：浮点数
- u：unit size
   - b: bytes
   - h: halfwords(two bytes)
   - w: words(four bytes, default)
   - g: giant word(eight words)
