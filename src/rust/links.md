### 资料

> 任何时候，如果你拿不准标准库中的类型或函数的用途和用法，不要慌或者只会 Google，请查阅应用程序接口（application programming interface，API）文档！

- 官网文档 [The Rust Programming Language](https://doc.rust-lang.org/stable/book/title-page.html)
    - 中文版：[https://kaisery.github.io/trpl-zh-cn](https://kaisery.github.io/trpl-zh-cn/title-page.html)
    - 互动学习版本：[https://rust-book.cs.brown.edu](https://rust-book.cs.brown.edu)
    - 本地离线版本：**rustup docs --book**
- 常用的 Rust 标准库: [std - Rust](https://doc.rust-lang.org/std/) 
- 图灵有本不错的书：[《Programming Rust: Fast, Safe Systems Development，2nd Edition》](https://www.oreilly.com/library/view/programming-rust-2nd/9781492052586/)
- Rust 语言圣经：[https://course.rs](https://course.rs/about-book.html)
- https://rust-book.junmajinlong.com/ch7/02_vec_capacity_reallocation.html

不喜欢读文档：

- [Introduction - Rust By Example](https://doc.rust-lang.org/rust-by-example/)
- [Rustlings code kata, learn by fixing tiny failing tests](https://github.com/rust-lang/rustlings)
- [Videos | ULTIMATE Rust Lang Tutorial](https://www.youtube.com/watch?v=784JWR4oxOI)
- [锈书｜学完 Rust 后，可以做些什么](https://rusty.course.rs/)
- [陈天 · Rust 编程第一课-极客时间](https://time.geekbang.org/column/intro/100085301)
- https://time.geekbang.org/column/article/718813

优秀的教程：

- https://fasterthanli.me/articles/a-half-hour-to-learn-rust
- https://stevedonovan.github.io/rust-gentle-intro/
- https://learning-rust.github.io/docs/why-rust/
- https://www.freecodecamp.org/news/rust-in-replit/
- https://www.programiz.com/rust
- https://www.educative.io/courses/learn-rust-from-scratch
- https://learn.microsoft.com/en-us/training/paths/rust-first-steps/
- https://www.oreilly.com/library/view/rust-programming-by/9781788390637/
- https://www.youtube.com/@RustVideos

built my Rust toolkit：https://www.youtube.com/watch?v=ifaLk5v3W90

### 学习教程和书籍

深入了解
- https://rust-cli.github.io/book/index.html
- https://rustwasm.github.io/docs/book/
- https://doc.rust-lang.org/embedded-book
- https://doc.rust-lang.org/reference/index.html
- https://doc.rust-lang.org/nomicon/index.html
- https://doc.rust-lang.org/nightly/unstable-book/index.html

- 《Rust设计模式》（Rust Design Patterns）也是一本开放书籍，着重于教给用户Rust的惯用法。
- Possible Rust是一个设计精美的语言站，谈论“Rust实际可能实现的事情”。主要提供两个主要部分：指南和模式。
- Easy Rust（dhghomon.github.io/easy_rust/），这是一本非常值得零基础的人入门的教程。这是另一本开放书籍，它以一种简单的方式解释和处理Rust的概念，使它们更易于理解和理解，尤其是对于那些初次接触Rust的人或来自其他高级语言的工程师。改善一些举例和类比非常适用，会让某些生涩的概念更好的理解，书中一些小例子也是非常好，比如对各种Rust类型的介绍例子，可以解决我们刚入Rust，不知道怎么下手写代码的情况。概述可以结合Rust官方的教程一起学习。喜欢看视频学习（虫虫强烈不建议）的同学也有个好消息，就是该书配套的视频教程也在油管上有了（youtube /playlist?list=PLfllocyHVgsRwLkTAhG0E-2QxCf-ozBkk）。

https://github.com/0voice/Understanding_in_Rust
https://bbs.huaweicloud.com/blogs/328048
https://baijiahao.baidu.com/s?id=1695541208019513535&wfr=spider&for=pc
https://blog.51cto.com/u_15683898/5407960
https://zhuanlan.zhihu.com/p/599522779?utm_id=0

Awesome 集合：
- https://github.com/rust-boom/rust-boom
- https://github.com/rust-unofficial/awesome-rust
- https://github.com/rustcc/awesome-rust
- https://github.com/vaaaaanquish/Awesome-Rust-MachineLearning
- https://github.com/rust-in-blockchain/awesome-blockchain-rust

---

[Cytoscape.js](https://js.cytoscape.org/)

Graph theory (network) library for visualisation and analysis

[Introduction - Learning Rust With Entirely Too Many Linked Lists](https://rust-unofficial.github.io/too-many-lists/index.html)

# 代码质量管控

## RCRG（Rust Code View Review Guidelines Rust ）代码审查指南

[请求贡献｜Rust 代码审查指南](https://mp.weixin.qq.com/s/u0fmDYGbMDLfZaIOwSjPmw)

[GitHub - ZhangHanDong/rust-code-review-guidelines: Rust Code Review Guidelines , RCRG](https://github.com/ZhangHanDong/rust-code-review-guidelines)

### UI 框架

- Makepad：https://mp.weixin.qq.com/s/BVJKlOdwmVi1a1NFrGN8Ng


Rust 生态
- [篇一 | 想全面了解 Rust 语言 ？ 你想知道的都在这里](https://mp.weixin.qq.com/s/F_38SD34nDl7cZYJqZFNww)
- [篇二 | 想全面了解 Rust 语言 ？ 你想知道的都在这里](https://mp.weixin.qq.com/s/YfoGpDtkF779hS3nDr9s8w)

- Rust 的 WASM，例如 `swc`、 `deno` 等。同时 `nextjs`
- WebGL/WebGPU
- 基础设施层，数据库、搜索引擎、网络设施、云原生等
- 系统开发（Linux 内核） / 系统工具（重写c、c++）
- 操作系统，谷歌的 Fuchsia，RISC-V
- 区块链、游戏 🎮


- 内存安全 Memory Safety，
    - 在 Rust 中，main 线程的栈大小是 8MB，普通线程是 2MB，在函数调用时会在其中创建一个临时栈空间，调用结束后 Rust 会让这个栈空间里的对象自动进入 Drop 流程，最后栈顶指针自动移动到上一个调用栈顶，无需程序员手动干预，因而栈内存申请和释放是非常高效的