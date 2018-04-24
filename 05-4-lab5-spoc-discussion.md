# 05-4-lab5-spoc-discussion

计44 黄锐皓 2014011385

## 视频相关思考题

### 14.1 总体介绍

1. 第一个用户进程创建有什么特殊的？

   用户态代码段的初始化。

2. 系统调用的参数传递过程？

   参见：用户态函数syscall()中的汇编代码。

   Ref: <https://www.ibm.com/developerworks/library/l-ia/index.html>

3. getpid的返回值放在什么地方了？

   参见：用户态函数syscall()中的汇编代码。

### 14.2 进程的内存布局

1. ucore的内存布局中，页表、用户栈、内核栈在逻辑地址空间中的位置？

   ```C
   memlayout.h

   #define VPT 0xFAC00000

   #define KSTACKPAGE 2 // # of pages in kernel stack

   #define KSTACKSIZE (KSTACKPAGE * PGSIZE) // sizeof kernel stack

   #define USERTOP 0xB0000000

   #define USTACKTOP USERTOP

   #define USTACKPAGE 256 // # of pages in user stack

   #define USTACKSIZE (USTACKPAGE * PGSIZE) // sizeof user stack
   ```


1. (spoc)尝试在panic函数中获取并输出用户栈和内核栈的函数嵌套信息和函数调用参数信息，然后在你希望的地方人为触发panic函数，并输出上述信息。
2. (spoc)尝试在panic函数中获取和输出页表有效逻辑地址空间范围和在内存中的逻辑地址空间范围，然后在你希望的地方人为触发panic函数，并输出上述信息。
3. 尝试在进程运行过程中获取内核空间中各进程相同的页表项（代码段）和不同的页表项（内核堆栈）？

### 14.3 执行ELF格式的二进制代码-do_execve的实现

1. 在do_execve中的的当前进程如何清空地址空间内容的？在什么时候开始使用新加载进程的地址空间？

   清空进程地址空间是在initproc所在进程地址空间。

​	CR3设置成新建好的页表地址后，开始使用新的地址空间。

2. 新加载进程的第一级页表的建立代码在哪？
3. do_execve在处理中是如何正确区分出用户进程和线程的？并为此采取了哪些不同的处理？

### 14.4 执行ELF格式的二进制代码-load_icode的实现

1. 第一个内核线程和第一个用户进程的创建有什么不同？

   相应线程的内核栈创建时，多了SS和ESP的设置；

   用户进程需要创建用户地址空间，并把用户代码复制到用户地址空间；

1. 尝试跟踪分析新创建的用户进程的开始执行过程？

### 14.5 进程复制

1. 为什么新进程的内核堆栈可以先于进程地址空间复制进行创建？

   内核栈在进程的内核地址空间，而各进程的内核地址空间是共享的；

2. 进程复制的代码在哪？复制了哪些内容？
3. 进程复制过程中有哪些修改？为什么要修改？

   内核栈，页表，trapframe，context，PCB字段修改。

4. 分析第一个用户进程的创建流程，说明进程切换后执行的第一条是什么。

### 14.6 内存管理的copy-on-write机制

1. 什么是写时复制？
2. 写时复制的页表在什么时候进行复制？共享地址空间和写时复制有什么不同？
3. 存在有多个（n>2）进程具有父子关系，且采用了COW机制的情况。这个情况与只有父子两个进程的情况相比，在设计COW时，需要注意的新问题是什么？有何解决方案？