# 05-1-spoc-discussion

计44 黄锐皓 2014011385

## 视频相关思考题

### 11.1 进程的概念

1. 什么是程序？什么是进程？
  - 程序：一系列为了某个目标的指令序列。
  - 进程：具有一定独立功能的程序在一个数据集合上的一次动态执行过程。

2. 进程有哪些组成部分？

   程序、数据、执行状态。

3. 请举例说明进程的独立性和制约性的含义。

   每个进程都相互独立运行，这是独立性；但是进程占用的资源由系统调度，互相制约，这是制约性。

4. 程序和进程联系和区别是什么？

   - 联系：程序是进程的一部分，一个程序可以对应多个进程
   - 区别：程序静态，进程动态；进程暂时，程序永久

### 11.2 进程控制块

1. 进程控制块的功能是什么？

   管理和控制进程运行。

2. 进程控制块中包括什么信息？

   - 进程基本信息：进程标识
   - 进程控制信息：调度、通信、资源占用
   - 运行状态：处理机现场

3. ucore的进展控制块数据结构定义中哪些字段？有什么作用？

### 11.3 进程状态

1. 进程生命周期中的相关事件有些什么？它们对应的进程状态变化是什么？
   - 抢占、唤醒、结束
   - 运行->就绪，等待->就绪，运行->退出

### 11.4 三状态进程模型

1. 运行、就绪和等待三种状态的含义？7个状态转换事件的触发条件是什么？
   - 运行：占用CPU执行指令
   - 就绪：拥有其他资源，满足运行条件，等待CPU资源
   - 等待：等待某事件或资源，不占用CPU，暂停
   - 触发条件：
     - 创建：启动
     - 创建->就绪：进入就绪队列
     - 就绪->运行：被调度
     - 运行->等待：等待事件
     - 运行->就绪：时间片用完
     - 等待->就绪：事件发生
     - 运行->退出：结束

### 11.5 挂起进程模型

1. 引入挂起状态的目的是什么？
   - 减少进程占用的内存，将进程部分移入外存
2. 引入挂起状态后，状态转换事件和触发条件有什么变化？
   - 就绪->就绪挂起或等待->等待挂起：挂起
   - 就绪挂起->就绪或等待挂起->等待：激活
3. 内存中的什么内容放到外存中，就算是挂起状态？
   - 进程内核栈被放到外存

### 11.6 线程的概念

1. 引入线程的目的是什么？

   在同一地址空间内实现并发的函数执行。

2. 什么是线程？

   线程是进程中描述指令流执行状态的组成部分，是CPU调度的基本单位。

3. 进程与线程的联系和区别是什么？

   - 进程是资源分配单位：进程地址空间、进程占用资源
   - 线程是CPU调度单位：CPU寄存器信息、堆栈信息、
   - 进程内的多个线程共享相同的地址空间

### 11.7 用户线程

1. 什么是用户线程？
   - 以用户函数库形式提供的线程实现机制
2. 用户线程的线程控制块保存在用户地址空间还是在内核地址空间？
   - 用户地址空间

### 11.8 内核线程

1. 用户线程与内核线程的区别是什么？

   - 用户线程是由函数库在用户态实现的线程机制；
   - 内核线程是由内核通过系统调用调用实现的线程机制；
   - 区别：实现方式、TCB的保存位置、运行开销、线程阻塞的影响范围

2. 同一进程内的不同线程可以共用一个相同的内核栈吗？

   可以。

3. 同一进程内的不同线程可以共用一个相同的用户栈吗？

   不可以。