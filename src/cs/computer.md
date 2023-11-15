# 计算机如何运转

想象一下，当你在浏览网页或者和朋友视频聊天的时，在漂亮的界面下面程序到底如何运行的一无所知。但是你决定成为一个软件工程师，你就要开始了解这些内部到底是如何运转的：

- 机器之间是如何通信的？数据传输和存储的方式？如何保证通信安全？
- 鼠标点击操作是如何变成一堆计算机执行指令的？
- 常听的那些计算机零件，芯片、CPU、GPU、内存等是如何协同工作的？...

虽然不用像硬件工程师熟悉集成电路设计，焊电路板，但是对于**计算机的组成、运转机制、程序编译过程**了然于胸、非常通透，绝对是有益无害的投资。

主要需要学习以下内容：

1）计算机组成（Computer Organization）：计算机组成强调的是有关控制信号（怎样控制计算机）、信号传递方式以及存储器类型等问题，涵盖了有关计算机系统的物理构成的各个方面。

   - 回答：**计算机是如何工作？**

2）计算机体系架构（Computer Architecture）：集中讨论计算机系统的结构和行为，主要涉及程序员熟悉的系统实现的逻辑方面内容。其包括许多基本要素，如指令集和指令格式、操作码、数据类型、寄存器的数目和类型、寻址方式、主存储器的访问方式和各种I/O机制等。对于某个特定的计算机，计算机体系结构就是其各硬件部分的组合，再加上其**指令集体系结构**（ISA，instruction set architecture）。

   - 回答：**怎样设计一台计算机？**

3）编译原理，编译原理是比较枯燥的一部分知识，包括词法分析、语法分析、语义分析、运行时环境、寄存器分配、代码优化与生成等内容。

4）操作系统（Operate System）我们下一节单独聊。

#### 学习课程和资源推荐

- CMU（卡内基梅隆大学）的镇系神课 Computer Systems: A Programmer's Perspective
  - 课程链接：[http://csapp.cs.cmu.edu/](http://csapp.cs.cmu.edu/)
  - B 站中英文视频：[https://www.bilibili.com/video/BV1iW411d7hd](https://www.bilibili.com/video/BV1iW411d7hd)

- CS61c，Berkeley 大学系列课程，深入计算机的硬件细节，吃透计算机体系架构：
  - [UCB CS61C: Great Ideas in Computer Architecture - CS自学指南](https://csdiy.wiki/%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84/CS61C/)

关于编译原理，机客时间的付费课程（非广告），适合工作几年的工程师补课: [编译原理之美](https://time.geekbang.org/column/intro/219), 时间和精力充足，也可以考虑啃斯坦福的 CS143 Compilers：

- [CS143: Compilers](https://web.stanford.edu/class/cs143/)

- [CS143 斯坦福大学编译原理【中文字幕】_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1NE411376V)

对于初学者来说，Compiler Construction: Principles and Practice 比龙书可能更友好一些（需要读英文版），《Parsing Techniques》也有很多人推荐，没了解过：

- [编译原理与实践](https://book.douban.com/subject/1100726/)

- [Parsing Techniques](https://book.douban.com/subject/4291903/)

如果想学习编译原理，不妨也了解一下: [LLVM Tutorial: Table of Contents — LLVM 15.0.0git documentation](https://llvm.org/docs/tutorial/index.html)
