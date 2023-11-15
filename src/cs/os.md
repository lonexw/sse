# 吃透操作系统

我们开发的软件并不能直接去控制硬件资源，需要找一个代理人/媒介去管理硬件资源和软件的交互，就像一个资源管理器（resource collector），这就是操作系统存在的意义。当我们按下开机键后，位于电脑主板上的固件 ROM 中的启动程序（Boot Loader）会运行，启动程序会加载操作系统的启动程序，进而把整个操作系统加载到内存中并开始执行操作系统。

> 操作系统也是一种软件，但是操作系统是一种非常复杂的软件。

操作系统提供了几种抽象模型:

> 文件：对 I/O 设备的抽象  ｜ 虚拟内存：对程序存储器的抽象

> 进程：对一个正在运行程序的抽象 ｜ 虚拟机：对整个操作系统的抽象

操作系统课程包含的主要内容：

- 操作系统运行时（程序中断、定时器）
- CPU 调度
- 进程（同步、互斥）、线程、管程（Monitor）
- 存储管理
- 文件系统
- I/O 输入
- 死锁
- 信号量编程

读书、看视频课程都可以加深对操作系统的了解，经典书籍：

- [爆肝上传！清华大佬终于把困扰我大学四年的【计算机操作系统】讲的如此通俗易懂_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1wq4y1M7qf?p=11)
- [现代操作系统（第3版）](https://book.douban.com/subject/3852290/)

不过我们也可以学习一些跟得上时代的内容：

- [两位图灵奖得主万字长文：新计算机架构，黄金十年爆发！](https://zhuanlan.zhihu.com/p/56018400)

#### 推荐课程 #1：MIT 6.S081: Operating System Engineering

MIT PDOS 实验室开设的面向本科生的操作系统课程，注重动手能力培养，让你在基于 RISC-V 开发的操作系统 xv6 之上增加新特性，深刻理解操作系统的每一部分。

[](https://pdos.csail.mit.edu/6.828/2021/overview.html)

- [book-riscv-rev2.pdf](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/125711BA-D177-4402-BF5E-06357B516253/94B8246C-EE28-43E1-9246-078496D0F902_2/eYIX7RBCXG6Wc5i7L8LNFTAT0Cs8dZcnF0SL2gB5UbYz/book-riscv-rev2.pdf)

#### 推荐课程 #2：rCoreOS 清华大学的操作系统课程

清华大学是国内首个使用 Rust 进行操作系统教学的高校。目前，陈渝教授和他的学生吴一凡正在编写新的操作系统教材。

- [rCore-Tutorial-Book 第三版 — rCore-Tutorial-Book-v3 3.6.0-alpha.1 文档](https://rcore-os.github.io/rCore-Tutorial-Book-v3/)

这本教程旨在一步一步展示如何 **从零开始** 用 **Rust** 语言写一个基于 **RISC-V** 架构的 **类 Unix 内核** 。值得注意的是，本项目不仅支持模拟器环境（如 Qemu/terminus 等），还支持在真实硬件平台 Kendryte K210 上运行。

> rust 社区也有一个重写操作系统的小项目，可以和这个项目做个对比参照：[Writing an OS in Rust](https://os.phil-opp.com/)，基于 **x86架构**（the x86 architecture），使用 Rust 语言，编写一个最小化的 64 位内核。

#### 推荐课程 #3： 南京大学`计算机系统基础`课程的项目 (Programming Assignment)

理解"程序如何在计算机上运行"的根本途径是从"零"开始实现一个完整的计算机系统. PA 将提出 x86/mips32/riscv32 架构相应的教学版子集, 指导学生实现一个经过简化但功能完备的 x86/mips32/riscv32 模拟器NEMU(NJU EMUlator), 最终在NEMU上运行游戏"仙剑奇侠传", 来让学生探究"程序在计算机上运行"的基本原理.

- [Introduction · GitBook](https://nju-projectn.github.io/ics-pa-gitbook/ics2019/)

学习自己动手构建一个操作系统，会让你对操作系统有全新的理解。