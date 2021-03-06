# lec1: 操作系统概述

---

## **提前准备**

（请在上课前完成）

* 完成lec1的视频学习和提交对应的在线练习
* git pull ucore\_os\_lab, ucore\_os\_docs, os\_tutorial\_lab, os\_course\_exercises in github repos。这样可以在本机上完成课堂练习。
* 知道OS课程的入口网址，会使用在线视频平台，在线练习/实验平台，在线提问平台\(piazza\)
  * [http://os.cs.tsinghua.edu.cn/oscourse/OS2019spring](http://os.cs.tsinghua.edu.cn/oscourse/OS2019spring)


* 会使用linux shell命令，如ls, rm, mkdir, cat, less, more, gcc等，也会使用linux系统的基本操作。
* 在piazza上就学习中不理解问题进行提问。



# 思考题

## 填空题

* 当前常见的操作系统主要用ASM,C,C++编程语言编写。
* "Operating system"这个单词起源于Operator 。
* 在计算机系统中，控制和管理资源 、有效地组织多道程序运行的系统软件称作操作系统 。
* 允许多用户将若干个作业提交给计算机系统集中处理的操作系统称为批处理操作系统
* 你了解的当前世界上使用最多的操作系统是Android（运行在32位CPU的ARM上） 。
* 应用程序通过系统调用接口获得操作系统的服务。
* 现代操作系统的特征包括并发性，共享性，虚拟性，异步性，持久性。
* 操作系统内核的架构包括宏内核 ，微内核 ，外核 。


## 问答题

- 请总结你认为操作系统应该具有的特征有什么？并对其特征进行简要阐述。
  - 并发性。由于同一时间可能有多个程序在运行，处理器在处理多道程序时要使得其在宏观上是同时运行的，实际内部可以完成分时交替运行。操作系统应该能够完成这多个系统交替运行的调度，比如什么时候中断，中断后从什么地方继续运行等问题，从而实现多个程序的并发运行调度。
  - 共享性。程序运行时会访问系统中的各种资源，操作系统应该能够控制实现每个程序对资源的互斥访问，并且保证内存可供并发的程序使用。
  - 虚拟性。操作系统要让用户觉得有一个计算机对其进行服务，要将物理上的实体转换为逻辑上的对应从而满足程序的使用要求。例如实现虚拟内存、虚拟处理器等等。
  - 异步性。由于处理器无法真正做到同时并发运行多个程序，需要利用分时交替的方法完成。所以每个程序的运行可能会走走停停，从而可能会使得进程产生和时间有关的错误。操作系统需要保证运行环境相同时，多次运行进程都有相同的运行结果。
  - 持久性。操作系统需要通过实现文件系统来持久储存程序以及相关的数据文件供系统使用。


- 为什么现在的操作系统基本上用C语言来实现？为什么没有人用python，java来实现操作系统？

  - C语言性能良好，是编译型语言，可以和汇编语言结合，从而可以很好地控制硬件的运行，对完成操作系统更有优势，所以操作系统基本上用C语言实现。
  - python和java相比之下没有C语言的性能优越，并且不能直接操作硬件，但是也存在利用python和java编写的操作系统例如JavaOS、pycorn。

---

## 可选练习题

---

- 请分析并理解[v9\-computer](https://github.com/chyyuu/os_tutorial_lab/blob/master/v9_computer/docs/v9_computer.md)以及模拟v9\-computer的em.c。理解：在v9\-computer中如何实现时钟中断的；v9 computer的CPU指令，关键变量描述有误或不全的情况；在v9\-computer中的跳转相关操作是如何实现的；在v9\-computer中如何设计相应指令，可有效实现函数调用与返回；OS程序被加载到内存的哪个位置,其堆栈是如何设置的；在v9\-computer中如何完成一次内存地址的读写的；在v9\-computer中如何实现分页机制。


- 请编写一个小程序，在v9-cpu下，能够输出字符


- 输入的字符并输出你输入的字符


- 请编写一个小程序，在v9-cpu下，能够产生各种异常/中断


- 请编写一个小程序，在v9-cpu下，能够统计并显示内存大小



- 请分析并理解[RISC-V CPU](http://www.riscvbook.com/chinese/)以及会使用模拟RISC\-V(简称RV)的qemu工具。理解：RV的特权指令，CSR寄存器和在RV中如何实现时钟中断和IO操作；OS程序如何被加载运行的；在RV中如何实现分页机制。
  - 请编写一个小程序，在RV下，能够输出字符
  - 输入的字符并输出你输入的字符
  - 请编写一个小程序，在RV下，能够产生各种异常/中断
  - 请编写一个小程序，在RV下，能够统计并显示内存大小

## 参考资料
 - [Intel格式和AT&T格式汇编区别](http://www.cnblogs.com/hdk1993/p/4820353.html)
 - [x86汇编指令集  ](http://hiyyp1234.blog.163.com/blog/static/67786373200981811422948/)
 - [PC Assembly Language, Paul A. Carter, November 2003.](https://pdos.csail.mit.edu/6.828/2016/readings/pcasm-book.pdf)
 - [*Intel 80386 Programmer's Reference Manual*, 1987](https://pdos.csail.mit.edu/6.828/2016/readings/i386/toc.htm)
 - [IA-32 Intel Architecture Software Developer's Manuals](http://www.intel.com/content/www/us/en/processors/architectures-software-developer-manuals.html)
 - [v9 cpu architecture](https://github.com/chyyuu/os_tutorial_lab/blob/master/v9_computer/docs/v9_computer.md)
 - [RISC-V cpu architecture](http://www.riscvbook.com/chinese/)
 - [OS相关经典论文](https://github.com/chyyuu/aos_course_info/blob/master/readinglist.md)
