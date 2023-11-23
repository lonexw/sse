# 选择一把合适的剑：Rust

### Why Rust?

选择主力的编程语言需要慎重，语言不仅仅是工具，而且是一种思维方式的改变。

Rust is a language for the next 40 years.

Rust is a universal language.

A new way for programming. Rust  makes you feel like a genius, also unsafe code for you, rust magic：code doesen‘t compile.

编译器是你最好的朋友，驾校教练。（强类型检查、GOOD ERRORS）

Your Code Can be Perfect. and Your Code WON'T Crash.
- 错误和边界处理
- 内存安全，无 GC
- Modern Tooling
- Think in expression ｜ Iterators ｜ Option ｜ Rich Types
- async code and macro
- Zero Cost Abstraction(CPU 概念的举例，只存在我们脑海，本质是 01)

喜欢吃螃蟹 🦀️，缓解内卷（不只当螺丝钉），**无 GC 且无需手动内存管理、性能高、工程性强、语言级安全性以及能同时得到工程派和学院派认可的语言。**

动机：我能够完成我的项目：
- FAST
- RELIABLE from the start
    - NO UNUSED VARIABLES
    - EXHAUSTIVE PATTERN MATCHING
    - ERRORS MUST BE HANDLED
- PRODUCTIVE：NO RUST2.0
    - Type System with Superpower
 
- Design Code 
   - Build Structure, Convert Business Knowledge to Type System/Enum
     - No Classes
     - ALGEBRAIC TYPE SYSTEM
     - data normalization
     - state machines
- Write Code / Read Code / Test Code
   - Work with compiler.
- Maintenance Code / Support their Code
    - Trust code, 少维护
>

### Roadmap 学习之旅 


Rust 之难，不在于语言特性，而在于：

- 实践中如何融会贯通的运用
- 遇到了坑时（生命周期、借用错误，自引用等）如何迅速、正确的解决
- 大量的标准库方法记忆及熟练使用，这些是保证开发效率的关键
- 心智负担较重，特别是初中级阶段

不能抱着试一试的态度，rust 的情况不允许你边工作，一边轻松愉快的学习，需要有计划的深入学习，系统性的提升编程能力，rust 无法看看语法就开始写代码，需要多学几次，深入进去，在克服诸多困难之后，会收获与众不同的编程之旅。

built my Rust toolkit：https://www.youtube.com/watch?v=ifaLk5v3W90

#### 1）编程语言的基础概念

- 变量、基本类型以及复合类型
- 重要的集合类型
- Function and Control Flow、
- Structs

#### 2）Rust 语言的难点

- 所有权、借用和引用
- 泛型、Traits及生命周期
- 循环引用和自引用问题


#### 3）更多语言能力

- 枚举与模式匹配，状态机，孙悟空72变
- 错误处理
- 全局变量
- 代码组织：Cargo、Package、Crates、注释及文档
- 自动化测试

https://www.youtube.com/watch?v=dFkGNe4oaKk

#### 4）进阶学习

- 函数式编程：闭包、迭代器
- 智能指针
- 多线程并发编程
- Unsafe Rust
- Marco 宏编程 https://www.youtube.com/watch?v=MWRPYBoCEaY
    - DSL & 元编程
    - They run at compile time
    - They can modify your sources code
    - MARCOS are a build tool inside your code
 - async/await 异步编程

#### 5）实战项目
- [Writing an OS in Rust](https://os.phil-opp.com/)
- [用Rust写操作系统 | rCore OS 教程介绍 - Rust精选](https://rustmagazine.github.io/rust_magazine_2021/chapter_1/rcore_intro.html)
- 用 Rust 写爬虫：https://github.com/lonexw/rust-crawler
- 键值数据库 kv-server：https://github.com/lonexw/kv-server
- Redis 和 web 服务：https://course.rs/advance-practice/intro.html

>

### 学习教程和书籍

深入了解
- https://rust-cli.github.io/book/index.html
- https://rustwasm.github.io/docs/book/
- https://doc.rust-lang.org/embedded-book
- https://doc.rust-lang.org/reference/index.html
- https://doc.rust-lang.org/nomicon/index.html
- https://doc.rust-lang.org/nightly/unstable-book/index.html

Awesome 集合：
- https://github.com/rust-boom/rust-boom
- https://github.com/rust-unofficial/awesome-rust
- https://github.com/rustcc/awesome-rust
- https://github.com/vaaaaanquish/Awesome-Rust-MachineLearning
- https://github.com/rust-in-blockchain/awesome-blockchain-rust

---

[Cytoscape.js](https://js.cytoscape.org/)

Graph theory (network) library for visualisation and analysis





### Performance

- Minimal Runtime
- Zero-Cost-Abstractions
- No Garbage Collector

### Memory Safety

- Ownership * Borrowing

在 Rust 中，main 线程的栈大小是 8MB，普通线程是 2MB，在函数调用时会在其中创建一个临时栈空间，调用结束后 Rust 会让这个栈空间里的对象自动进入 Drop 流程，最后栈顶指针自动移动到上一个调用栈顶，无需程序员手动干预，因而栈内存申请和释放是非常高效的。

unsafe rust

### Type System

- Immutability & Privacy By Default
- No Null Values: **::Option  (Some(T) or None)::**
- Explicit Error Handling: **::enum Result ( Ok(T) or Err(E))、unwrap::**
- Pattern Matching
- Closures
- Iterators & Combinators
- Enums & Structs, user-defined types.

Generic Types 泛型 & Trait & Lifetime

Trait

impl Trait for Struct， default impl

Trait Object

Assoacited Trait





### Polymorphism

- No Classical Inheritance
- Traits
- Generics

# Functional Language Features: Iterators and Closures

闭包可以从环境中捕获值，与函数接受参数的方式是完全一致的：获取所有权、可变借用及不可变借用，编码表现为 3 种 Trait：

- ::FnOnce::：意味着闭包可以从它的封闭作用域中，也就是闭包所处的环境中，消耗捕获的变量。为了实现这一功能，闭包必须在定义时取得这些变量的所有权并将它们移动至闭包中，因为闭包**不能多次获取并消耗同一变量的所有权，所以只能被调用一次（Once）**。
- ::FnMut::：可以从环境中可变地借用值并对它们进行修改。
- ::Fn::：可以从环境中不可变地借用值。

> 当你创建闭包时，Rust会基于闭包从环境中使用值的方式来自动推导出它需要使用的trait。所有闭包都自动实现了FnOnce，因为它们至少都可以被调用一次。那些不需要移动被捕获变量的闭包还会实现FnMut，而那些不需要对被捕获变量进行可变访问的闭包则同时实现了Fn。

> 使用 **::move::** 关键字，强制闭包获取环境中值的所有权。

### Iterator 迭代器

在Rust中，迭代器是惰性的（layzy）。这也就意味着创建迭代器后，除非你主动调用方法来消耗并使用迭代器，否则它们不会产生任何的实际效果。

> iter() | iter_into() | iter_mut()

迭代器可以让开发者专注于高层的业务逻辑，而不必陷入编写循环、维护中间变量这些具体的细节中。



### Extensibility

- With Macros

### Building & Package Management

- cargo | crates.io
- rustup | docs

```typescript
$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

运行期初始化静态变量

使用`lazy_static`在每次访问静态变量时，会有轻微的性能损失，因为其内部实现用了一个底层的并发原语`std::sync::Once`，在每次访问该变量时，程序都会执行一次原子指令用于确认静态变量的初始化是否完成

[https://github.com/rust-lang-nursery/lazy-static.rs](https://github.com/rust-lang-nursery/lazy-static.rs)

[Introduction - Learning Rust With Entirely Too Many Linked Lists](https://rust-unofficial.github.io/too-many-lists/index.html)

# 代码质量管控

## RCRG（Rust Code View Review Guidelines Rust ）代码审查指南

[请求贡献｜Rust 代码审查指南](https://mp.weixin.qq.com/s/u0fmDYGbMDLfZaIOwSjPmw)

[GitHub - ZhangHanDong/rust-code-review-guidelines: Rust Code Review Guidelines , RCRG](https://github.com/ZhangHanDong/rust-code-review-guidelines)

### UI 框架

- Makepad：https://mp.weixin.qq.com/s/BVJKlOdwmVi1a1NFrGN8Ng


#### 关于 Rust 错误的信息,比你想知道的更多


引用 Rust Book 中的话：“错误是软件生活中的一个事实”。这篇文章讲更细致地讨论如何处理它们。很值得一读


- [https://www.shuttle.rs/blog/2022/06/30/error-handling](https://www.shuttle.rs/blog/2022/06/30/error-handling)

一个非常简单的例子，访问数据数组的方式。itemCount[n],rust 需要你覆盖所有错误

# **making perfect software**

your code can be oerfect

- Deep familiarity with Rust abstractions, memory management, and concurrency

内存安全，以及垃圾回收 ♻️：所有权和借用检查，为了让编译器立即程序，需要丰富的类型系统（enum、struct）。

OPtion and Result 随处可见、零成本抽象、迭代

High Level Features / Low Level SPEED

async code Work Done ｜ wasm、contontainer、嵌入式

also modern tooling：cargo、fmt、test、bench、clippy、rustup

building tools：macro 宏、unsafe expression

Basic：variable binding、type annotation、元组、_、断言、block are expressiom

Middle： match expersion、struct / impl for struct、struct / function can be generic

High：macro、Option、Result、iterators

单元：crate、



---

创业失败的破产工程师，我基本上算是没有面试经历，也没有参与太多的大型项目，所以我的思考可以辩证的来看，非经验之谈，而是一种思考框架。

审视如何做软件，如何做人，如何生活。

No LeetCode, no take-home assignments, etc.

到了一定年龄，专注是克服时间本身的唯一途径。

软件行业的细分岗位太多，我只探索了我想从事或者感兴趣的一些领域，有很多比如游戏开发，嵌入式开发我都兴趣较弱或者没有接触过，如果你感兴趣可以自行探索一下。

[Things Are Getting Rusty In Kernel Land](https://hackaday.com/2022/05/17/things-are-getting-rusty-in-kernel-land/)


对设计和某些专业有更深的理解，综合性人才会更出色。MUI’s material design components

性能，理解内存、堆栈、引用、变量作用域等，以及编译器的强迫症。

学习自驱性。

- Rust 的 WASM，例如 `swc`、 `deno` 等。同时 `nextjs`
- WebGL/WebGPU
- 基础设施层，数据库、搜索引擎、网络设施、云原生等
- 系统开发（Linux 内核） / 系统工具（重写c、c++）
- 操作系统，谷歌的 Fuchsia，RISC-V
- 区块链、游戏 🎮

Rust 生态
- [篇一 | 想全面了解 Rust 语言 ？ 你想知道的都在这里](https://mp.weixin.qq.com/s/F_38SD34nDl7cZYJqZFNww)
- [篇二 | 想全面了解 Rust 语言 ？ 你想知道的都在这里](https://mp.weixin.qq.com/s/YfoGpDtkF779hS3nDr9s8w)

Python
- https://www.razorsecure.com/careers
- https://www.datacamp.com
Haskell
- Learn Your a Haskell for Great Good
- Real World Haskell
