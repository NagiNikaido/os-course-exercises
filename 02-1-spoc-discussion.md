# 02-1-spoc-discussion

计44 黄锐皓 2014011385

## 第三讲 启动、中断、异常和系统调用-思考题

### 3.1 BIOS

- BIOS从磁盘读入的第一个扇区是是什么内容？为什么没有直接读入操作系统内核映像？

BIOS从磁盘中读入的第一个散去时MBR，主引导扇区。主引导扇区记录硬盘本身的相关信息，在读取它之前无法在硬盘中找到操作系统的数据。

- 比较UEFI和BIOS的区别。

  UEFI启动对比BIOS启动的优势有三点：

  - 安全性更强：UEFI启动需要一个独立的分区，它将系统启动文件和操作系统本身隔离，可以更好的保护系统的启动。
  - 启动配置更灵活：EFI启动和GRUB启动类似，在启动的时候可以调用EFIShell，在此可以加载指定硬件驱动，选择启动文件。比如默认启动失败，在EFIShell加载U盘上的启动文件继续启动系统。
  - 支持容量更大： 传统的BIOS启动由于MBR的限制，默认是无法引导超过2TB以上的硬盘的。随着硬盘价格的不断走低，2TB以上的硬盘会逐渐普及，因此UEFI启动也是今后主流的启动方式。

### 3.2 系统启动流程

- 分区引导扇区的结束标志是什么？

0X55AA

- 在UEFI中的可信启动有什么作用？

利用数字签名来确认EFI驱动程序或者应用程序是否是受信任的。

### 3.3 中断、异常和系统调用比较

- 什么是中断、异常和系统调用？

中断：外部意外的响应

异常：指令执行意外的响应

系统调用：系统调用指令的响应

- 中断、异常和系统调用的处理流程有什么异同？

源头上，中断来源于外设，异常源于执行时的意外，系统调用来源于应用程序的请求。处理机制上，中断对于应用程序是透明的，异常将杀死或重新执行意想不到的应用程序指令。

- 以ucore lab8的answer为例，uCore的系统调用有哪些？大致的功能分类有哪些？

Linux的系统调用大概有250个左右，其继承了UNIX系统调用中最基本和最有用的部分，按照功能逻辑大致可以分为如下几类：

1. 进程控制：创建、终止、挂起进程等基本操作以及进程信息的查询
2. 文件系统控制：打开、关闭、读写文件等基本操作以及更改文件属性等系统操作
3. 系统控制：系统内核相关信息的查询以及一些系统级别的控制操作
4. 内存管理：分配、释放内存、地址映射、缓冲写回等内存相关的操作
5. 网络管理：域名的查询和设置以及主机名称、标识号的查询与设置
6. socket控制：建立连接、发送消息、断开连接等一系列套接字操作
7. 用户管理：用户和组相关信息的设置和查询
8. 进程间通信：进程间的消息、信号、管道、共享内存等通信操作

### 3.4 linux系统调用分析

- 通过分析[lab1_ex0](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex0.md)了解Linux应用的系统调用编写和含义。(仅实践，不用回答)
- 通过调试[lab1_ex1](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex1.md)了解Linux应用的系统调用执行过程。(仅实践，不用回答)

### 3.5 ucore系统调用分析 （扩展练习，可选）

- 基于实验八的代码分析ucore的系统调用实现，说明指定系统调用的参数和返回值的传递方式和存放位置信息，以及内核中的系统调用功能实现函数。
- 以ucore lab8的answer为例，分析ucore 应用的系统调用编写和含义。
- 以ucore lab8的answer为例，尝试修改并运行ucore OS kernel代码，使其具有类似Linux应用工具`strace`的功能，即能够显示出应用程序发出的系统调用，从而可以分析ucore应用的系统调用执行过程。

### 3.6 请分析函数调用和系统调用的区别

- 系统调用与函数调用的区别是什么？

指令不同，特权级不同，堆栈切换

- 通过分析`int`、`iret`、`call`和`ret`的指令准确功能和调用代码，比较函数调用与系统调用的堆栈操作有什么不同？