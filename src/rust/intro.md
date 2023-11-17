# 选择一把合适的剑：Rust

## Why Rust?

> A new way for programming.

喜欢吃螃蟹 🦀️，缓解内卷（不只当螺丝钉），**无 GC 且无需手动内存管理、性能高、工程性强、语言级安全性以及能同时得到工程派和学院派认可的语言。**

Rust 之难，不在于语言特性，而在于：

- 实践中如何融会贯通的运用
- 遇到了坑时（生命周期、借用错误，自引用等）如何迅速、正确的解决
- 大量的标准库方法记忆及熟练使用，这些是保证开发效率的关键
- 心智负担较重，特别是初中级阶段

不能抱着试一试的态度，rust 的情况不允许你边工作，一边轻松愉快的学习，需要深入学习，系统性的提升编程能力，rust无法看看语法就开始写代码，需要多学几次，深入进去，在克服诸多困难之后，会收获与众不同的编程之旅。

性能，理解内存、堆栈、引用、变量作用域等，以及编译器的强迫症。

学习自驱性。

- Rust 的 WASM，例如 `swc`、 `deno` 等。同时 `nextjs`
- 基础设施层，数据库、搜索引擎、网络设施、云原生等
- 系统开发（Linux 内核） / 系统工具（重写c、c++）
- 操作系统，谷歌的 Fuchsia，RISC-V
- 区块链、游戏 🎮

### Performance

- Minimal Runtime
- Zero-Cost-Abstractions
- No Garbage Collector

### Memory Safety

- Ownership * Borrowing

### Type System

- Immutability & Privacy By Default
- No Null Values: **::Option  (Some(T) or None)::**
- Explicit Error Handling: **::enum Result ( Ok(T) or Err(E))、unwrap::**
- Pattern Matching
- Closures
- Iterators & Combinators
- Enums & Structs, user-defined types.

### Polymorphism

- No Classical Inheritance
- Traits
- Generics

### Extensibility

- With Macros

### Building & Package Management

- cargo | crates.io
- rustup | docs

```typescript
$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

### 运行期初始化静态变量

使用`lazy_static`在每次访问静态变量时，会有轻微的性能损失，因为其内部实现用了一个底层的并发原语`std::sync::Once`，在每次访问该变量时，程序都会执行一次原子指令用于确认静态变量的初始化是否完成

[https://github.com/rust-lang-nursery/lazy-static.rs](https://github.com/rust-lang-nursery/lazy-static.rs)

[Introduction - Learning Rust With Entirely Too Many Linked Lists](https://rust-unofficial.github.io/too-many-lists/index.html)


