#### ProcessBase (进程的基本概念)

1. 进程

    - 对用户来看，进程就是一个正在运行的程序
    - 对操作系统而言，进程就是一个名为task_struct的结构体，一般也称之为进程控制块(PCB Process Control Block)，他里面包括了该进程的状态、性质、资源以及组织。
2. 进程的状态

    linux进程状态有5种，他们是在内核中这样定义的：
    ```c
    #define TASK_RUNNING              0 
    #define TASK_INTERRUPTIBLE        1
    #define TASK_UNINTERRUPTIBLE      2
    #define TACK_ZOMBIE               4
    #define TACK_STOPPED              8
    ```
- TASK_RUNNING状态：该状态并不是表示一个进程正在执行，或者说是这个进程就是”当前进程“，而是表示这个进程可以被调度成为当前进程。当进程处于这个状态时，内核就会讲该进程的task_struct结构体通过队列头run_list挂入一个”运行队列“。

- TASK_INTERRUPTIBLE和TASK_UNINTERRUPTIBLE状态：这两个状态都是表示进程处于睡眠状态。但是后者是处于”深度睡眠“而不会受到信号(signal,也称为软中断)的打扰，而前者可以因”信号“的到来而唤醒。

- TASK_ZOMBIA状态：表示该进程已经”死亡”，但是其task_struct还在，等待父进程处理。

- TASK_STOPPED状态：主要用于调试。进程收到一个SIGSTOP信号后，就将运行状态改为TASK_STOPPED状态而“挂起”，然后接收到一个SIGCONT信号后，又可以恢复运行。

3. 进程状态之间的转换及相应的函数
![](./ProcessStatusTransfer.jpg)

4. task_struct结构体
```c

```

