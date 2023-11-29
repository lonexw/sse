# 选择一把合适的剑 🗡️：Rust

> 选择主力的**编程语言**需要慎重，语言不仅仅是工具，还是一种思维方式。


Rust 是一门赋予每个人构建**可靠且高效软件能力**的语言，最初目的是用来编写以往都是由 C/C++ 编写的高性能程序(这类程序非常容易出现内存安全问题), 现在你已经可以在[很多领域](./rust/rust_in_production.md)看到它的身影, 从初创公司到大型企业，从嵌入式设备到可扩展的 Web 服务，Rust 都完全适用。

### 语言特性

- Performance 高性能
    - Minimal Runtime
    - No Garbage Collector 垃圾回收 ♻️
    - Zero-Cost-Abstractions
- 安全可靠
    - Immutability & Privacy By Default
    - Ownership model guarantee memory-safety
    - thread-safety
    - No Null Values: **::Option  (Some(T) or None)::**
- Type System, Enum, Match
- Modern Tooling 
- 函数式编程：Closures、Iterators & Combinators
- Async code and macro

用我的 Expound Note 或 figma 来输出图例；

### 构建健壮的软件

1. Rust 很有前途, A new way for programming, 学会就是合格的程序员，值的长期投入。
2. 在编译器的指导下，可以写更好、不会轻易 Crash（错误和边界处理） 的软件。
3. How to Design Code 的跳转。

喜欢吃螃蟹 🦀️，缓解内卷（不只当螺丝钉），**无 GC 且无需手动内存管理、性能高、工程性强、语言级安全性以及能同时得到工程派和学院派认可的语言。**

Rust 是图灵完备的通用编程语言, 也是未来几十年的主流编程语言，它更加安全，更不容易崩溃，更容易编写、维护和调试，让程序员可以写出更安全，高效的代码。

A new way for programming. Rust  makes you feel like a genius, also unsafe code for you.

Your Code Can be Perfect. and Your Code WON'T Crash.

错误和边界处理，eliminate many classes of bugs at compile-time.

- rust magic：code doesen‘t compile.
- 为了让编译器理解程序，需要丰富的类型系统（enum、struct）
- 编译器是你最好的朋友，驾校教练。（强类型检查、GOOD ERRORS）

    
- RELIABLE from the start
    - NO UNUSED VARIABLES
    - EXHAUSTIVE PATTERN MATCHING
    - ERRORS MUST BE HANDLED
- PRODUCTIVE：NO RUST2.0
    - Type System with Superpower

High Level Features / Low Level SPEED


### Roadmap 学习之旅 

0）回顾一些计算机基础知识：

- 计算机内存管理、指针、编译器原理

Rust 之难，不在于语言特性，而在于：

- 实践中如何融会贯通的运用
- 遇到了坑时（生命周期、借用错误，自引用等）如何迅速、正确的解决
- 大量的标准库方法记忆及熟练使用，这些是保证开发效率的关键
- 心智负担较重，特别是初中级阶段

不能抱着试一试的态度，rust 的情况不允许你边工作，一边轻松愉快的学习，需要有计划的深入学习，系统性的提升编程能力，rust 无法看看语法就开始写代码，需要多学几次，深入进去，在克服诸多困难之后，会收获与众不同的编程之旅。


#### 1）编程语言的基础概念

- 变量、基本类型以及复合类型
- 重要的集合类型
- Function and Control Flow、
- Structs、泛型、Traits

#### 2）Rust 语言的难点

为何不用赋值而用绑定呢（其实你也可以称之为赋值，但是绑定的含义更清晰准确）？这里就涉及 Rust 最核心的原则——所有权

- 所有权、借用和引用
- 生命周期、全局变量
- 循环引用和自引用问题
- 指针、切片类型


#### 3）工程能力

- 错误处理
- 代码组织：Cargo、Package、Crates、注释及文档
- 自动化测试

Building & Package Management

https://www.youtube.com/watch?v=dFkGNe4oaKk

#### 4）进阶学习

- 函数式编程：闭包、迭代器
- 智能指针
- 多线程并发编程
- Unsafe Rust
- Marco 宏编程, Extensibility
 https://www.youtube.com/watch?v=MWRPYBoCEaY
    - DSL & 元编程
    - They run at compile time
    - They can modify your sources code
    - MARCOS are a build tool inside your code
 - async/await 异步编程

### 项目实战
- [Writing an OS in Rust](https://os.phil-opp.com/)
- [用Rust写操作系统 | rCore OS 教程介绍 - Rust精选](https://rustmagazine.github.io/rust_magazine_2021/chapter_1/rcore_intro.html)
- 用 Rust 写爬虫：https://github.com/lonexw/rust-crawler
- 键值数据库 kv-server：https://github.com/lonexw/kv-server
- Redis 和 web 服务：https://course.rs/advance-practice/intro.html
