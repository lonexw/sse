# 常见的编程概念

### 变量绑定、解构与可变性

变量绑定（variable binding），为什么不叫变量赋值[后面](../ownership.md)会聊到：
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

**Enums 枚举**

列举所有可能的成员（变体 Variant）来定义的数据类型，在 Rust 中非常有用且常见。

不少现实世界的事务（可穷举）都可以用枚举来表示（月份、星期、方向等）。

```rust
#[derive(Debug)]
enum Message {                  // 定义枚举类型来表示系统消息
    Quit,                       // 无数据的枚举成员
    Move { x: i32, y: i32 },    // 包含匿名结构体
    Write(String),              // 包含 String 字符串
    ChangeColor(i32, i32, i32), // 包含元组结构体
    Other,                      // 其他消息数据
}
let msg = Message::Write("Start Success!");

#[derive(Copy, Clone)]
#[repr(u8)]   // 限定范围为`0..=255`
enum Week {   // 定义英文的星期和数值相对应的枚举
  Monday = 1, // 1
  Tuesday,    // 2
  Wednesday,  // 3
  Thursday,   // 4
  Friday,     // 5
  Saturday,   // 6
  Sunday,     // 7
}

impl Week {   // 为枚举类型定义方法 Methods
  fn is_weekend(&self) -> bool {
    if (*self as u8) > 5 {
      return true;
    }
    false
  }
}

let mon = Week::Monday as i32;  // 使用 as 将 enum 成员转换为对应的数值。

// 用 Rust 实现 Json 解析工具，定义枚举类型去枚举 Json 允许的数据类型
// 在其他语言中，可能需要定义很多方法来表达出这些内容
enum Json {
  Null,
  Boolean(bool),
  Number(f64),
  String(String),
  Array(Vec<Json>),
  Object(Box<HashMap<String, Json>>),
}
```

常见的枚举 **Option**，`Some or None` 只是省略了 `Option::` 前缀, 本质上是枚举值;
```rust
enum Option<T> {
    Some(T),    // Some(T) 可以包含任何类型的数据
    None,       // 表示空值
}
```

对 Option<T> 进行运算之前必须将其转换为 T（unwrap 方法）。通常这能帮助我们捕获到空值最常见的问题之一：假设某值不为空但实际上为空的情况，消除了错误地假设一个非空值的风险，会让你对代码更加有信心。

这是 Rust 的经过深思熟虑的设计决策，来限制空值的泛滥以增加 Rust 代码的安全性。

- 标准库文档 [std-Enums](https://doc.rust-lang.org/std/option/enum.Option.html)

> 枚举类型和下面的模式匹配控制流是绝佳拍档。

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


### 模式匹配（Patern match）

match 控制流称得上是神兵利器,为复杂的类型系统提供了非常棒的解构能力。就像绝世锦囊，**确保所有可能的情况都得到处理**，算无遗策。

无论是多个 `if-else` 的条件分支判断场景，还是根据枚举类型的值匹配执行不同的任务，match 模式匹配都可以有更高效、简练的代码表达(可以少写不少代码)。

```rust
// Option 的本质是一个枚举值，有两个成员：Some<T> | None
let r: Option<&str> = Some("Rust");   
println!("{}", r.unwrap());

// unwrap 的实现就是利用了 macth, 等同于:
match r {                           // 对 Option 进行模式匹配
    Some(i) => println!("{i}"),     // 处理不同枚举值的执行，此处 Some 内部的值绑定了变量 i：&str
    None => println!("None")        // 必须处理所有情况，匹配是穷尽的
} // match 也是表达式，会返回值, 可以使用 macth 来赋值
```
再看一个示例，比如我们玩掷骰子 🎲 游戏，如果结果是小于等于 3, 会失去 1 积分，如果结果是 6 会获得 2 积分，其他情况积分不变：

```rust
match dice_roll {
    1 | 2 | 3 => lose(),    // 或者范围表达式：1..=3
    6 => win(),
    _ => do_nothing(),  // _ 占位符代表其他的值，满足穷举性
}
```
macthes! 宏与变量遮蔽的细节：
```rust
enum MyEnum { Foo, Bar }

fn main() {
    let v = vec![MyEnum::Foo,MyEnum::Bar,MyEnum::Foo];
    v.iter().filter(|x| matches!(x, MyEnum::Foo));

    let foo = 'f';
    assert!(matches!(foo, 'A'..='Z' | 'a'..='z'));

    let bar = Some(4);
    assert!(matches!(bar, Some(x) if x > 2));

    // match or if-let 都是新的代码块，此处绑定相当于新变量
    let age = Some(30);
    match age {
        // 此处绑定相当于 let age = 30; 原先的 age: Some(30) 被覆盖了 
        Some(age) =>  println!("将 Some 内部数据（{}）绑定到变量 age", age),
        _ => ()
    }
}
``` 
蛮多情况，我们只想处理匹配某个模式的情况，而不关心其他匹配情况，可以简写程序：
```rust
// 只处理有配置数值的情况
let config_max = Some(3u8);
match config_max {
    Some(max) => println!("The maximum is configured to be {}", max),
    _ => (),
}

// 用 if-let 控制流简写
if let Some(max) = config_max {
    println!("The maximum is configured to be {}", max);
}
```
简单解释一下，`let PATTERN = EXPERSION` 变量绑定是一种最简单的模式匹配，将表达式与模式匹配，并对匹配成功高的进行变量绑定（赋值），回想一下变量绑定的知识：
```rust
let x = 1;
let (x, y) = (0, 1);    // 元组的解构赋值，x=0,y=1

// 现在是不是更容易上面理解 if-let 操作啦，同理，while-let 也没啥问题
let mut stack = vec![1, 2, 3];
while let Some(top) = stack.pop() { // let 模式匹配，变量绑定 top
  println!("{}", top);  // 没有元素 pop, 返回 None,模式无法匹配，退出循环
}

// for (idx, value) in ids.iter().enumerate() {} 
// for 迭代同样也是模式匹配的过程，给控制变量绑定赋值

fn add(x: i32, y:i32) {}// 函数参数传值，本质也是在做模式匹配(参数绑定)的操作
```
咋样，模式匹配非常有用吧，看透本质，学习神速 ⚡

#### 模式匹配的高级用法

- 解构后进行模式匹配时，如果某个值没有对应的变量名，则可以使用 @ 手动绑定一个变量名;
- ref 和 mut 修饰模式匹配中的变量（所有权和可变性）;
- 匹配守卫（match guard），允许匹配分支添加额外的后置条件;
- 解构赋值时，如果解构的是一个引用，则被匹配的变量也将被赋值为对应元素的引用;
- 对解引用（deref）进行匹配时, 可能会发生所有权转移;（所有权的难点）

---

### References

- 关键字：https://kaisery.github.io/trpl-zh-cn/appendix-01-keywords.html
- 命名规范：https://course.rs/practice/naming.html