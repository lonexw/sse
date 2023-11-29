# ❌ 错误处理

不好的代码编写策略：
- try catch
- GOTO
- You Can‘t fix NULLS，NO NULLS

Railway Oriented Programming：
https://fsharpforfunandprofit.com/rop/

Write Code That Never Crashes
- Options and Result
- 使用 Enum 枚举自定义错误分支

#### 关于 Rust 错误的信息,比你想知道的更多


引用 Rust Book 中的话：“错误是软件生活中的一个事实”。这篇文章讲更细致地讨论如何处理它们。很值得一读


- [https://www.shuttle.rs/blog/2022/06/30/error-handling](https://www.shuttle.rs/blog/2022/06/30/error-handling)

一个非常简单的例子，访问数据数组的方式。itemCount[n],rust 需要你覆盖所有错误