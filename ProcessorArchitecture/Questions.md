1. 请简述精简指令集RISC(Reduced Instruction-Set Computer)和复杂指令集CISC(complex instruction set computer)之间的区别
> 上世纪70年代IBM的John Cocke发现，处理器提供的大量指令集和复杂寻址方式其中只有20%会被经常用到，通常简单指令大都能在一个cycle内完成，基于该思想的指令集叫RISC，以前的叫CISC;
> RISC处理器通过更合理的维架构在性能上打败了CISC处理器，后来Intel设计了Pentium Pro处理器，x86指令集被解码成类似RISC指令的微操作指令(micro-operations,简称uops),
> 以后执行的过程采用RISC内核的方式，导致其性能逐渐超过同期的RISC.

2. 请简述数值0x12345678在大(Big-endian)小(Little-endian)端字节序处理器的存储器中的存储方式
> 计算机系统中是以字节为单位的，每一块内存空间对应着一个字节(8个比特位)，由于寄存器宽度大于一个字节，所以必然存在如何安排多个字节的问题;
> 大小端记忆(小小小，即低字节放在低地址为小端); 如何检测:联合体Union的存放顺序是所有成员都从低地址开始放的
```c
int checkCPU(void)
{
    union w
    {
        int a;
        char b;
    }c;
    c.a = 1;
    return (c.b == 1);
}
```
> 如果返回true，则为小端；否则为大端

3. 请简述在你所熟悉的处理器(比如双核Cortex-A9)中一条存储读写指令的执行全过程
> 经典处理器架构的流水线是五级流水线：取指、译码、发射、执行和写回

> 现代处理器在设计上都采用了超标量结构(Superscalar Architecture)和乱序执行(out-of-order)技术，极大地提高了处理器计算能力
> 超标量技术能在一个时钟周期(CPU主频的倒数)内执行多个指令，实现指令级的并行，有效提高ILP(Instruction Level Parallelism)指令级的并行效率，同时也增加了这个cache和memory层次结构的实现难度

> 一条存储读写指令的执行全过程：
> 对于写指令：
> - 指令首先进入流水线(pipeline)的前端(Front-End)，包括预取(fetch)和译码(decode)，经过分发(dispatch)和调度(scheduler)后进入执行单元，最后提交执行结果。
> - 所有的指令采用顺序方式(In-Order)通过前端，并采用乱序的方式(Out-of-Order, OOO)进行发射，然后乱序执行，最后用顺序方式提交结果，并将最终结果更新到LSQ(Load-Store Queue)部件
> - - 总的来说：**顺序提交指令，乱序执行(非真正数据依赖和地址依赖的指令)，最后顺序提交结果**；如果有两条没有数据依赖的数据指令，后面那条指令的读数据先被返回，它的结果也不能先写回到最终寄存器，而是必须等到前一条指令完成后才可以
> - LSQ部件是指令流水线的一个执行部件，可以理解为存储子系统的最高层，其上接收来自CPU的存储器指令，其下连接着存储子系统。主要功能是将来自CPU的存储器请求发送到存储器子系统，并处理其下存储器子系统的应答数据和消息
> 对于读指令：
> - 当处理器在等待数据从缓存或者内存返回时，对于乱序执行的处理器，可以执行后面的指令；对于顺序执行的处理器，会使流水线停顿，直到读取的数据返回

![Image discription](https://github.com/RocketKernel/LinuxKernelGo/blob/master/pic/x86mpu.png)

