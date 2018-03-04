# 01-3-lab0-spoc-discussion

计44 黄锐皓 2014011385

## 思考题

- 你理解的对于类似ucore这样需要进程/虚存/文件系统的操作系统，在硬件设计上至少需要有哪些直接的支持？至少应该提供哪些功能的特权指令？

硬件支持：串口控制器，块设备控制器，存储控制亲，终端设备，磁盘和磁带，物理内存。

特权指令：涉及外部设备的输入输出指令，修改特殊寄存器的指令，改变机器状态的指令。

- 你理解的x86的实模式和保护模式有什么区别？物理地址、线性地址、逻辑地址的含义分别是什么？

**实模式和保护模式：**

1. 可访问的物理内存空间大小不同。实模式下可访问的物理内存空间不超过1MB，保护模式下可寻址4G字节的物理地址空间。
2. 根本区别是进程内存是否受保护，实模式将整个物理内存看成分段的区域，程序代码和数据位于不同区域，系统程序和用户程序没有区别对待，而且每一个指针都是指向"实在"的物理地址。而保护模式下，物理内存地址不能直接被程序访问，程序内部的地址（虚拟地址）要由操作系统转化为物理地址去访问，对程序透明。

**地址：**

物理地址：处理器提交到总线上用于访问计算机系统中的内存和外设的最终地址。

线性地址：逻辑地址到物理地址变换之间的中间层，处理器通过段机制控制下的形成的地址空间。

逻辑地址：在有地址变换功能的计算机中,访问指令给出的地址。

- 理解list_entry双向链表数据结构及其4个基本操作函数和ucore中一些基于它的代码实现（此题不用填写内容）
- 对于如下的代码段，请说明":"后面的数字是什么含义

```C
 /* Gate descriptors for interrupts and traps */
 struct gatedesc {
    unsigned gd_off_15_0 : 16;        // low 16 bits of offset in segment
    unsigned gd_ss : 16;            // segment selector
    unsigned gd_args : 5;            // # args, 0 for interrupt/trap gates
    unsigned gd_rsv1 : 3;            // reserved(should be zero I guess)
    unsigned gd_type : 4;            // type(STS_{TG,IG32,TG32})
    unsigned gd_s : 1;                // must be 0 (system)
    unsigned gd_dpl : 2;            // descriptor(meaning new) privilege level
    unsigned gd_p : 1;                // Present
    unsigned gd_off_31_16 : 16;        // high bits of offset in segment
 };
```

每一个域在结构体中所占的位数，表示这个成员变量占多少位。

- 对于如下的代码段，

```C
#define SETGATE(gate, istrap, sel, off, dpl) {            \
    (gate).gd_off_15_0 = (uint32_t)(off) & 0xffff;        \
    (gate).gd_ss = (sel);                                \
    (gate).gd_args = 0;                                    \
    (gate).gd_rsv1 = 0;                                    \
    (gate).gd_type = (istrap) ? STS_TG32 : STS_IG32;    \
    (gate).gd_s = 0;                                    \
    (gate).gd_dpl = (dpl);                                \
    (gate).gd_p = 1;                                    \
    (gate).gd_off_31_16 = (uint32_t)(off) >> 16;        \
}

```

如果在其他代码段中有如下语句，

```C
unsigned intr;
intr=8;
SETGATE(intr, 1,2,3,0);
```

请问执行上述指令后， intr的值是多少？

0x10002