1. 请简述精简指令集RISC(Reduced Instruction-Set Computer)和复杂指令集CISC(complex instruction set computer)之间的区别
> 上世纪70年代IBM的John Cocke发现，处理器提供的大量指令集和复杂寻址方式其中只有20%会被经常用到，通常简单指令大都能在一个cycle内完成，基于该思想的指令集叫RISC,以前的叫CISC;
> RISC处理器通过更合理的维架构在性能上打败了CISC处理器，后来Intel设计了Pentium Pro处理器，x86指令集被解码成类似RISC指令的微操作指令(micro-operations,简称uops),
> 以后执行的过程采用RISC内核的方式，导致其性能逐渐超过同期的RISC.

2. 请简述数值0x12345678在大(Big-endian)小(Little-endian)端字节序处理器的存储器中的存储方式
> 计算机系统中是以字节为单位的，每一块内存空间对应着一个字节(8个比特位)，由于寄存器宽度大于一个字节，所以必然存在如何安排多个字节的问题;
> 


