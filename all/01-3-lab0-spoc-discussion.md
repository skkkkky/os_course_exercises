# lec2：lab0 SPOC思考题

## **提前准备**
（请在上课前完成，option）

- 完成lec2的视频学习
- git pull ucore_os_lab, os_tutorial_lab, os_course_exercises  in github repos。这样可以在本机上完成课堂练习。
- 了解代码段，数据段，执行文件，执行文件格式，堆，栈，控制流，函数调用,函数参数传递，用户态（用户模式），内核态（内核模式）等基本概念。思考一下这些基本概念在不同操作系统（如linux, ucore,etc.)与不同硬件（如 x86, riscv, v9-cpu,etc.)中是如何相互配合来体现的。
- 安装好ucore实验环境，能够编译运行ucore labs中的源码。
- 会使用linux中的shell命令:objdump，nm，file, strace，gdb等，了解这些命令的用途。
- 会编译，运行，使用v9-cpu的dis,xc, xem命令（包括启动参数），阅读v9-cpu中的v9\-computer.md文档，了解汇编指令的类型和含义等，了解v9-cpu的细节。
- 了解基于v9-cpu的执行文件的格式和内容，以及它是如何加载到v9-cpu的内存中的。
- 在piazza上就学习中不理解问题进行提问。

---

## 思考题

- 你理解的对于类似ucore这样需要进程/虚存/文件系统的操作系统，在硬件设计上至少需要有哪些直接的支持？至少应该提供哪些功能的特权指令？
  + 进程：硬件上要能够支持时钟中断。需要提供中断的控制使能，能够触发中断的特权指令。
  + 虚存：硬件上需要有地址的映射，需要有内存管理单元MMU。需要提供能够设置寻址方式和页表管理的特权指令
  + 文件系统：硬件上需要有稳定的储存介质和输入输出的接口。需要提供对储存介质进行I/O的特权指令。
- 你理解的x86的实模式和保护模式有什么区别？你认为从实模式切换到保护模式需要注意那些方面？
  - 两模式的根本区别是进程内存是否受保护。其中实模式有16位寻址空间，采用分段的方式来把程序和数据划分到不同的内存区域而没有对系统程序和用户程序进行区分，没有采用虚拟地址空间，直接使用物理地址。保护模式有32位寻址空间，采用分页的方式来进行虚存管理，不能直接访问物理地址，需要由操作系统把虚拟地址转换成物理地址，从而保护了系统程序。
  - 首先需要注意寻址空间的大小发生了变化，最重要的是一个采用物理地址空间一个采用虚拟地址空间，保护模式需要实现好分页机制，操作系统要能够正确的完成虚拟地址到物理地址的转换。
- 物理地址、线性地址、逻辑地址的含义分别是什么？它们之间有什么联系？
  - 逻辑地址是应用程序直接使用的地址空间。
  - 线性地址是由于虚存管理每个应用程序所访问的地址空间，是段机制下形成的地址空间。
  - 物理地址是处理器提交到总线上用户访问计算机系统中的内存和外设的最终地址。
  - 应用程序直接访问逻辑地址，逻辑地址通过段机制转换成线性地址，线性地址再通过页机制转换成物理地址，之后再去访问实际的内存或者外设。
- 你理解的risc-v的特权模式有什么区别？不同 模式在地址访问方面有何特征？
  - RISC-V有四级特权模式，分别是机器模式M-mode、用户模式U-mode、管理员模式S-mode、Hypervisor模式H-mode。每个模式有多个控制和状态寄存器
  - M模式权限最高，可以不受限制的访问机器，H模式是支持虚拟机监视器而设置的，S模式和U模式分别用于支持操作系统和应用程序，处理M模式，其他模式中都对机器访问进行了一定的限制来保护系统的其他部分。
- 理解ucore中list_entry双向链表数据结构及其4个基本操作函数和ucore中一些基于它的代码实现（此题不用填写内容）
- 对于如下的代码段，请说明":"后面的数字是什么含义
```
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

数字表示结构体中的每个元素所占有的bit数。

- 对于如下的代码段，

```
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
```
unsigned intr;
intr=8;
SETGATE(intr, 1,2,3,0);
```
请问执行上述指令后， intr的值是多少？

答：intr的值为0x20003

### 课堂实践练习

#### 练习一

1. 请在ucore中找一段你认为难度适当的AT&T格式X86汇编代码，尝试解释其含义。

2. (option)请在rcore中找一段你认为难度适当的RV汇编代码，尝试解释其含义。

#### 练习二

宏定义和引用在内核代码中很常用。请枚举ucore或rcore中宏定义的用途，并举例描述其含义。

+ 利用宏访问复杂数据结构中的数据
+ 利用宏进行数据转换，比如to_struct
+ 常用功能的代码片段优化


## 问答题

#### 在配置实验环境时，你遇到了那些问题，是如何解决的。

## 参考资料
 - [Intel格式和AT&T格式汇编区别](http://www.cnblogs.com/hdk1993/p/4820353.html)
 - [x86汇编指令集  ](http://hiyyp1234.blog.163.com/blog/static/67786373200981811422948/)
 - [PC Assembly Language, Paul A. Carter, November 2003.](https://pdos.csail.mit.edu/6.828/2016/readings/pcasm-book.pdf)
 - [*Intel 80386 Programmer's Reference Manual*, 1987](https://pdos.csail.mit.edu/6.828/2016/readings/i386/toc.htm)
 - [IA-32 Intel Architecture Software Developer's Manuals](http://www.intel.com/content/www/us/en/processors/architectures-software-developer-manuals.html)
 - [v9 cpu architecture](https://github.com/chyyuu/os_tutorial_lab/blob/master/v9_computer/docs/v9_computer.md)
 - [RISC-V cpu architecture](http://www.riscvbook.com/chinese/)
 - [OS相关经典论文](https://github.com/chyyuu/aos_course_info/blob/master/readinglist.md)

