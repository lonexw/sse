# 常见的编程概念

### 变量绑定、解构与可变性

变量绑定（variable binding），为什么不叫变量赋值后面会聊到：
```rust
// &str 明确指定变量的类型，也可省略让编译器自行推导
let name: &str = "Rust"; 
// 也可以先声明(declare)，再初始化值(initialize)
let name: &str; // 没有初始化之前是不允许使用
name = "Rust";
// 重新绑定会覆盖（Shadowing）之前的值
let name = "Rust Lang";
// 解构式绑定，先简单了解
let (name, age) = ("Anonymous", 23);
println!("Name: {}, Age: {}", name, age);
```

常见操作是去改变一个变量的值：
```rust
let name = "Rust";
name = "Rust Lang"; // Err 会编译出错
```
在 Rust 中变量默认是 **🙅 不可变(immutable)**的，这可以带来两个好处：
- 语言层级的安全性和简单并发性
- 变量声明为不可变在运行期可以避免多余的 runtime 检查，提升性能

不用担心会因此损失灵活性，需要时手动增加 **mut** 声明即可：
```rust
let mut name = "Rust"; // mut: mutable 可变的
name = "Rust Lang"; // It's ok.
println!("Name is {}", name);
```

如需一个完全不允许改变的值，可以使用**常量(constants)**:
```
// 常量只能被设置为常量表达式，而不可以是其他任何只能在运行时计算出的值
// 常量可以作为多处代码使用的全局范围的值, 包括一些需要硬编码的值
const SPEED_OF_LIGHT = 299792458;
```

### 数据类型

Rust 是静态类型（statically typed）语言, 在编译时就必须知道所有变量的类型，编译器通常可以推断出我们想要用的类型。

#### 标量（Scalar）：代表一个单独的值

| 标量   | 说明   | 备注   |
|---------|---------|----------|
| 整型   | 有符号和无符号整数    | Decimal (十进制): 98_222 <br />Hex (十六进制): 0xff<br />Octal (八进制) : 0o77<br />Binary (二进制): 0b1111_0000<br />Byte (单字节字符)(仅限于u8) : b'A'  |
| 浮点型  | f32 和 f64   |  避免在浮点数上测试相等性   |
| 布尔类型  | true 和 false   |  let f: bool = false; // with explicit type annotation   |
| 字符类型  | `char`   |  字符只能用 '' 来表示， "" 是留给字符串  |

- 所有数字类型都支持基本数学运算：加法(+)、减法(-)、乘法(*)、除法(/)和取余(%), 其他[运算符](https://kaisery.github.io/trpl-zh-cn/appendix-02-operators.html)。
- [A collection of numeric types and traits for Rust](https://crates.io/crates/num)

#### 复合类型 Compound Types：多个值组合成一个类型

**Tuple 元组**类型：将多个不同类型的值组合进一个复合类型
- 元组长度固定：一旦声明，其长度不会增大或缩小。
- `()` 表示空值或空的返回类型。如果表达式不返回任何其他值，则会隐式返回单元值。
```rust
let language = "The Rust Language";
let tup: (&str, usize) = (language, language.len());
// . 操作符来获取元组中的值。
println!("The language is: {}", tup.0);
```

**Array 数组**类型：数组中的每个元素的类型必须相同
- 数组在栈 (stack) 上分配的已知固定大小的单个内存块，**长度固定**
```rust
// 数组类型说明语法：[&str; 12]
let months: [&str; 12] = ["January", "February", "March", "April", "May", "June", "July",
              "August", "September", "October", "November", "December"];

// 使用索引来访问数组的元素, 从 0 开始计数，越界访问会导致编译报错，避免非法的内存访问
let dec = months[11];
println!("Now is {}", dec);

// 数组切片 slice，从数组中 Array 从切出子数组
let the_first_season: &[&str] = &months[0..=2]; // 也可以简化为：&months[..3]
println!("The first season: {:?}", the_first_season);
```

### 函数与流程控制

Rust 是一门基于 **表达式（expression-based）** 的语言（函数式语言的重要特征），需要理解其区别。

程序是由一行行指令编写而成，把每一行代码区分为：
- 语句 Statements：执行一些操作但不返回值的指令；
- 表达式 Expressions：计算并产生一个值；
- 代码注释 Comments

```rust
// main 函数是程序的入口点，等同于 main() -> (), 无返回值
fn main() {
    let res = add(1, 1); // 函数调用是表达式，会求值
    println!("1 + 1 = {}", res); 

    // 大括号创建的一个新的块作用域也是一个表达式
    let z = {
        let x = 3;  // 等同于 let x = { 3 };
        x + 1
    };
}
// fn 关键字用来声明新函数，函数名 add, 参数 x、y, 返回值类型 i32
fn add(x: i32, y: i32) -> i32 {
    x + y // 表达式，等同于 return x + y; 语句
          // 如果在此加上 ; 就成了一个语句，整个函数其实会返回 ()，产生报错
}
```

根据条件来运行不同代码的是大部分编程语言的基本组成部分，常见的流程控制语法, 也都是表达式：

```rust
let result = if condition { // 这样赋值时需要保证每个赋值返回的值是同类型
    // A...
} else { // 或 else if
    // B...
}

// 循环执行：for / while / loop
// for 并不会使用索引去访问数组，安全且简洁，同时避免运行时的边界检查，性能更高
for i in 1..=5 { 
    if i == 2 {
        continue; // 跳过当次循环，break 可以跳出终止整个循环
    }
    println!("{}", i); 
}
for item in container { } // 循环中修改元素，使用 mut 关键字: &mut container
for (index, value) in array.iter().enumerate() // 获取数组中元素的索引

// 也可以用 loop + if 来组合实现
while n <= 5  {
    println!("{}!", n);
    n = n + 1;
}
```

### 枚举与模式匹配

Enums

match expression

if let :https://kaisery.github.io/trpl-zh-cn/ch06-03-if-let.html

### References

- 关键字：https://kaisery.github.io/trpl-zh-cn/appendix-01-keywords.html
- 命名规范：https://course.rs/practice/naming.html