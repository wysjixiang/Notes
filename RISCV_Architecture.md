



#### 中断处理

1. RISCV标准中对外部中断采用了共享方式。RISCV架构中所有的中断和异常都会转向mtvec寄存器所指向的地址，即所有的中断和异常都共享一个中断处理函数。进入共享中断处理函数之后才会进一步判断中断或异常的类型来进一步处理。
   1. 首先会通过mstatus寄存器判断是异常还是中断。
   2. 异常通过相应的异常号来进行相应的处理；
   3. 判别为中断的话，则需要进一步通过mcause寄存器来判别不同的中断种类，从而进行相应的处理
2. Machine模式下常用的控制状态寄存器CSR（直接看汪辰老师的PPT）
   1. mhartid：Hardware Thread ID。硬件线程的ID号
   2. mstatus：发生异常时的处理器状态
   3. mie：用于进一步控制（打开和关闭）software interrupt/timer interrupt/external interrupt
   4. mtvec：Machine Trap-Vector Base-Address。用来保存发生异常时需要跳转的地址
   5. mscratch：Machine 模式下专用寄存器，我们可以自己定义其用法，譬如用该寄存器保存当前在 hart 上运行的 task 的上下文（context）的地址。
   6. mepc：当trap发生时，将当时的指令地址（pc）保存到mepc寄存器中
   7. mcause：当trap发生时，由Hart（即硬件）来设置该寄存器来通知发生trap的原因。mcause的最高位interrupt标识是否为中断，如果该位是1,则是中断，否则是异常。其余的exception code则标识具体的中断或者异常种类。
   8. mtval：它保存了 exception 发生时的附加信息：譬如访问地址出错时的地址信息、或者执行非法指令时的指令本身，对于其他异常，它的值为0。
   9. mip：（machine interrupt pending）已经发生等待处理的异常