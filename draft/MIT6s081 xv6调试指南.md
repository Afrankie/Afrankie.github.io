---
title: "MIT6s081 xv6调试指南"
date: 2022-02-23T17:34:34+08:00
---


### 进入调试环境

配置.gdbinit

进入项目根目录，一个终端启动gdb服务，`make qemu-gdb`
进入项目根目录，另一个终端启动gdb客户端连接待调试程序，`riscv64-unknown-elf-gdb`


### 给程序打断点
gdb启动时停止的第一个位置位于内核(这块是通过什么控制的)？使用的内核状的符号表，所以不能在用户态的程序打断点，需要切换成用户态的符号表。
使用
```bash
# 切换为用户态某一文件的符号表
file user/_xxxx
# 然后可以直接使用行号打断点
b line_no
```
```bash
# 切换为内核态符号表
file kernel/kernel
# 使用文件名:行号打断点
b sysfile.c:1
```

### 查看变量
常用的几个命令
`info reg`
`info locals`
`x/5i \*addr`
`print/x $t0`


出现error reading variable: dwarf2_find_location_expression: Corrupted DWARF expression问题
将makefile中的-ggdb替换为-gdwarf-2


### 代码跳转指令
`si`
汇编指令级别
`n`
下一代码行
`s`
进入函数内部，不是函数则下一代码行
`c`
跳转到下一个断点位置

### 小技巧
调试时可以注释一些代码，缩小定位问题的范围。
比如新写了一段代码出现了bug，把它注释和不注释掉， bug都没有解决，那么可以先注释掉，简化问题访问，方便找到原因。