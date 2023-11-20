# CPU Architecture 架构

### 依旧在使用的 CPU 架构

- x86_64, 大部分计算机使用
- 基于 ARM64 的 **ISA（指令集架构）**
- MIPS：大多数路由器使用
- 基于`S/390`的大型机（现在改名为`z/Architecture`）
- 嵌入式系统
   - AVR（Arduino 设备）
   - SuperH（土星、Dreamcast、卡西欧9860计算器）
   - `805`
- RISC-V

定义特征上的区别：

- 字的大小（word size）。8、16、31、32、64位
- 设计风格（design style）。RISC（指令少，操作简单），CISC（指令多，执行复杂的操作，VLIW（指令长，同时并行做很多事情）
- 存储架构（memory architecture）
- 许可成本：RISC-V是开放的，可以免费使用
- 特性集（features set）：有些特性在特定架构平台有特定的支持。比如，浮点数（x87）、加密（AES-NI）、支持本地高级字节码执行（Jazelle、AVR32B）、矢量计算（SSE、AVX、AltiVec）。

### 动手制作 CPU

一些有趣的 homemade CPUs 项目

[Virtual x86](http://copy.sh/v86/)

[JSLinux](https://bellard.org/jslinux)

[Ben Eater](https://eater.net/8bit/)

[GitHub - darklife/darkriscv: opensouce RISC-V cpu core implemented in Verilog from scratch in one night!](https://github.com/darklife/darkriscv)

逻辑电路设计工具 🔧

[GitHub - logisim-evolution/logisim-evolution: Digital logic design tool and simulator](https://github.com/logisim-evolution/logisim-evolution)

[GitHub - hneemann/Digital: A digital logic designer and circuit simulator.](https://github.com/hneemann/Digital)

I/O Ports

[I/O Ports - OSDev Wiki](https://wiki.osdev.org/I/O_Ports)

[BLASTER Variable](https://dos.fandom.com/wiki/BLASTER_Variable)

`MMIO`（内存映射的输入/输出）

![Image.png](https://mmbiz.qpic.cn/mmbiz_png/hicrFibaKFMd3nc7ZzdTH8pkftcn1ibDzhX9hWVFg4cESYeHfkCDTZopS9SACEUkMAeRaxRHzzcXHeHXibEF0fUEyQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

#### 简单寄存器、指令

16 个 32 位的寄存器，编号 r0 到 r15:

- r0 - r7: 低位寄存器，常用位置
- r8-r15: 高位寄存器，只能用特定的指令进行操作
- r13：栈指针 sp - Stack Pointer
- r14:  链接寄存器 lr - Link Register
- r15: 程序计数器 pc - Program Counter

指令分类，每一类都包含一个相同的头部（common header）：

- ALU （算术与逻辑）运算
- load/store 指令
- 栈操作指令
- 分支指令（有条件和无条件）

#### 总线 Buses

三态逻辑来实现的：一个信号要么是`0`，要么是`1`，要么是`Z`（发音为 "高阻抗"）。`Z`是 “较弱”的信号，即，如果你将`Z`信号与`0`或`1`信号相连，将输出后者。

#### 电路设计实例

如 `{direction}R{sign}{mode} {destination}, [{base}, {offset}` 指令的组件：

- `{direction}`：不是 `LD(load)`，就是 `ST(store)`
- `{sign}`：要么不做（不扩展），要么就是 `S`（符号扩展值，以填充32位）
- `{mode}`：要么是 `nothing`（全字，32位）和 `H`（半字，16位），要么是 `B`（字节，8位）
- `{destination}`：目标寄存器，要读出/写入
- `{base}`, `{offset}`：内存中的地址（将是两者的值之和）

ldrh r1, [r2, r3] 指令的编码示意：

![Image.png](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/7CA026BB-0093-4C36-B4AA-DF6AC6558398/B4FB2ECF-1BE7-4EFC-A8A2-15FB6D4796A3_2/RUAaiWjmZMaJKAag4uvWqqb3LOA66ZNCLKWuWcagAqYz/Image.png)

`opcode` 是一个 3位的值，用于同时对`{direction}`, `{sign}` 和 `{mode}`进行编码。

电路图：

![Image.png](https://mmbiz.qpic.cn/mmbiz_png/hicrFibaKFMd3nc7ZzdTH8pkftcn1ibDzhXZg2O4gYvhQMqBZ322ibu1MBTosCXUt7uR4Y4YPfoCD9BCKuXXmQ2icLw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

- 三个寄存器（`Rd, Rb, Ro`）在各自的位置（`0-2, 3-5, 6-8`）从指令中被读取，并被送到相应的全局通道（`RW, RA, RB`）。
- `opcode`被解码，以检查它是一个`store`（`000，001，010`）还是一个`load`（剩余值）。
- `opcode`被解码以找到模式的值。`0`代表字，`1`代表半字，`2`代表字节。
- `opcode`再次被解码以找到符号的值（仅对操作码`011`和`111`来说是真的，所以我们可以检查最后两个比特是高位）。

Instr`0708`隧道是这个组件的激活引脚；如果当前指令属于这个指令组，它就是高电平。

### 内存 Memory

> CPU的语言是汇编指令，这些指令有一个固定的、定义好的编码。

内存操作的简化：`{`“`load value from address", "store value at address"}`.

**内存的工作原理**

![Image.png](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/7CA026BB-0093-4C36-B4AA-DF6AC6558398/745EBB78-57B6-4B6A-A4CB-566AA30239DA_2/BCEGlyUC0gWFut84dNOKmDbpwcl3poKx1xc7DLG18L0z/Image.png)

全局内存需要在任何时候要求任何数量的字节，是动态的，以堆（Heap）的形式存在。

本地内存用栈（Stack）的方式增长和收缩，有 push （增长）和 pop（缩小）操作，不需要对分配的内存块做任何登记，唯一需要关注的是该堆栈的深度，也就是栈的长度。通常的做法我们在内存中的某个地方设置为栈的起点，并在某个地方（例如，在一个寄存器中）保留一个全局变量，**该变量包含栈最顶层的项（topmost item）在内存中的位置：栈指针**（在ARM上为`sp`，或其全名为`r13`）。

栈的增长方向。

**::寻址模式和内存对齐::**

如果你想访问栈上的内存，你通常会访问栈顶部的东西（例如你的局部变量），所以你不必给出完整的内存地址（大），而只需要给出**相对于栈指针的数据距离**（小）。::这就是`sp-relative`寻址模式::，看起来像`ldr r1, [sp, #8]`。

假设你大多会存储`4`字节或更大的东西，所以我们会说栈是字对齐（word-aligned）的：所有东西都会被移动，以便地址是`4`的倍数。

### 函数调用 Function Call

在汇编中，调用函数的最简单方法是通过使用`jump`

如何解决跳过去，如何回来的问题：

使用一个全局变量（即寄存器）来存储调用者的地址，并有一个特殊的跳转指令，将寄存器设置到当前位置（链接），这样我们就可以在以后回到它（分支）。在ARM上，这就是bl（branch-link）系列指令，该寄存器被称为链接寄存器（缩写为`lr`，昵称为`r14`）

   - 对嵌套调用不起作用! 从另一个被调用的函数里面调用一个函数，值会被覆盖。
   - 当进入一个函数时，在栈中为局部变量分配空间，但也为必须保留的寄存器分配空间，当退出时，原始值从栈中放回到寄存器中。

### 设备 Devices

向地址`0xFFFFFF00`写一个字节将在终端显示器上显示一个字符。

从地址`0xFFFFFF18`中读取一个字节，就可以知道键盘缓冲区是否为空。

### 参考资料

[Crabs All the Way Down: Running Rust on Logic Gates](https://zdimension.fr/crabs-all-the-way-down/)


