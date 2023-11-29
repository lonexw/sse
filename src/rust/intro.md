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

- Rust 使用**所有权和借用模型**来保证内存安全（Memory safety），同时也通过 unsafe 代码块来提供完全的灵活性，还没有 GC（Garbage Collector 垃圾回收机制）带来的性能损耗。
- 为了让编译器（Compiler）深层次的理解你的代码，**强类型系统（Rich Type System）** 的支持就必不可少，编译器像你的老师或驾校教练，将更多的软件缺陷拦截在编译阶段。
    - 这是 Rust 的魔法 🪄：`Code doesen‘t compile.`, 不要让它成为你学习之路的拦路虎。
    - 在编译器的指导下，可以写更好、不会轻易 Crash（错误和边界处理） 的软件。
- Rust 从函数式语言的重要特征学习了不少东西：基于 `表达式（expression-based）` 、函数闭包（Closures）与迭代器（Iterators）, 以及强大的枚举（无处不在的 Option），可以让你即拥有高级语言和功能，又不丢失低级语言的性能，而且这些都是零成本抽象的（Zero-Cost Abstract）：无需为这些抽象付出成本。
- 强大的 Macro System（宏系统）也带来一个好处：语言版本迭代带来的学习和迁移痛苦。
- Rust 提供了完整的生态工具链和社区：
    - Cargo 依赖管理 📦 
    - cargo fmt: 代码格式化 ｜ cargo clippy: 代码检查 🧐
    - cargo test: 单元测试、文档测试 ｜ cargo bench: 基础测试
    - rustup: Rust 安装、环境、版本切换等

### 🦀️ Rust 代表一种全新的编程之路

掌握 Rust 不代表你的智商比他人高，但可以作为合格程序员的证书。

具备高性能、高可靠、高生产力的能力，拥有高级语言和低级语言的优势，适用于绝大多数行业领域，无畏并发，良好的语言生命力和社区生态，Rust 必将成为未来几十年的主流编程语言，值的长期投入。


#### 写出更好的代码

- RELIABLE from the start
    - NO UNUSED VARIABLES 
    - EXHAUSTIVE PATTERN MATCHING
    - ERRORS MUST BE HANDLED
- Your Code WON'T Crash.
- Rust  makes you feel like a genius, also unsafe code for you.
- PRODUCTIVE - Type System with Superpower
- 缓解内卷（不只当螺丝钉）

#### Rust 之难，不在于语言特性，而在于：

- 实践中如何融会贯通的运用
- 遇到了坑时（生命周期、借用错误，自引用等）如何迅速、正确的解决
- 大量的标准库方法记忆及熟练使用，这些是保证开发效率的关键
- 心智负担较重，特别是初中级阶段

不能抱着一直试一试的态度，Rust 的情况不允许你边工作，一边轻松愉快的学习，需要有计划的深入学习，系统性的提升编程能力，rust 无法看看语法就开始写代码，需要多学几次，深入进去，在克服诸多困难之后，会收获与众不同的编程之旅。

### Roadmap 学习之旅 

0）回顾一些计算机基础知识：

- 计算机内存管理、垃圾回收
- 指针、编译器原理
- 操作系统和网络编程


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
- async/await 异步编程
- Unsafe Rust
- Marco 宏编程, Extensibility
 https://www.youtube.com/watch?v=MWRPYBoCEaY
    - DSL & 元编程
    - They run at compile time
    - They can modify your sources code
    - MARCOS are a build tool inside your code

### 项目实战
- [Writing an OS in Rust](https://os.phil-opp.com/)
- [用Rust写操作系统 | rCore OS 教程介绍 - Rust精选](https://rustmagazine.github.io/rust_magazine_2021/chapter_1/rcore_intro.html)
- 用 Rust 写爬虫：https://github.com/lonexw/rust-crawler
- 键值数据库 kv-server：https://github.com/lonexw/kv-server
- Redis 和 web 服务：https://course.rs/advance-practice/intro.html
