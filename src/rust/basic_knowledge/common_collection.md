# 常用的集合 Collections

除了基本数据类型或固定大小的数组或元组存储特定的值，Rust 标准库还提供不少非常有用的集合数据结构来**存放动态数据**。

### 1）动态数组 Vector 
 
Vec<T> 存放多个相同类型的值：

```rust
// `Vec` is a regular struct，为了后续可以修改 vector, 上面加上了可变声明 mut
// 换种写法亦可：let v = vec![]; , 不要被宏 ！写法吓到
// 如可以预估数组长度，可以在初始化时指定 Vec::with_capacity(5);
let mut v: Vec<i32> = Vec::new();	
v.push(0);			// 更新 vector
v.extend([1, 2]);		// 拼接 vector

for i in &mut v { *i += 1; } 	// 可以循环遍历， *i 解引用获取值

let len = v.len();		// 获取 vector 长度
let last: i32 = v.pop()		// 这里获取了可变引用，如果不理解，学习了所有权章节后再回顾
		.unwrap();		// 并对 pop 方法返回 Option 枚举直接 unwrap
let one: i32 = v[0];		// 读取 vector 中的元素, 等同于 v.get(0)，不过要注意返回 Result
println!("Length of this vector is {len}. First is：{one}. Last is {last}");
```
巧妙利用枚举来存放**不同数据类型**的值：
```rust
#[derive(Debug)]
enum TableCell {	// 定义一个枚举类型表示电子表格的单元格数据
	Int(i32),		// 可能是数字
	Float(f64),		// 可能是浮点值
	Text(String),	// 可能是字符串
}

let row = vec![TableCell::Int(3), TableCell::Float(3.14), TableCell::Text("Rust".to_string())];

for (index, cell) in row.iter().enumerate() { 
	let pos = index + 1;
	println!("Cell {pos} is {:?} .", cell); 
}
```

初始化 vec 的更多方式：
```rust
fn main() {
    let v = vec![0; 3];   // 默认值为 0，初始长度为 3
    let v_from = Vec::from([0, 0, 0]);
    assert_eq!(v, v_from);
}
```
动态数据都在内存空间存放在堆（Heap）上，在内存中彼此相邻排列。

动态数组意味着我们增加元素时，如果**容量不足就会导致 vector 扩容**（目前的策略是重新申请一块 2 倍大小的内存，再将所有元素拷贝到新的内存位置，同时更新指针数据），显然，当频繁扩容或者当元素数量较多且需要扩容时，大量的内存拷贝会降低程序的性能。

可以考虑在初始化时就指定一个实际的预估容量，尽量减少可能的内存拷贝：
```rust
fn main() {
    let mut v = Vec::with_capacity(10);
    v.extend([1, 2, 3]);    // 附加数据到 v
    println!("Vector 长度是: {}, 容量是: {}", v.len(), v.capacity());

    v.reserve(100);        // 调整 v 的容量，至少要有 100 的容量
    println!("Vector（reserve） 长度是: {}, 容量是: {}", v.len(), v.capacity());

    v.shrink_to_fit();     // 释放剩余的容量，一般情况下，不会主动去释放容量
    println!("Vector（shrink_to_fit） 长度是: {}, 容量是: {}", v.len(), v.capacity());
}
```
Vector 常见的一些方法示例：
```rust
let mut v =  vec![1, 2];
assert!(!v.is_empty());         // 检查 v 是否为空

v.insert(2, 3);                 // 在指定索引插入数据，索引值不能大于 v 的长度， v: [1, 2, 3] 
assert_eq!(v.remove(1), 2);     // 移除指定位置的元素并返回, v: [1, 3]
assert_eq!(v.pop(), Some(3));   // 删除并返回 v 尾部的元素，v: [1]
assert_eq!(v.pop(), Some(1));   // v: []
assert_eq!(v.pop(), None);      // 记得 pop 方法返回的是 Option 枚举值
v.clear();                      // 清空 v, v: []

let mut v1 = [11, 22].to_vec(); // append 操作会导致 v1 清空数据，增加可变声明
v.append(&mut v1);              // 将 v1 中的所有元素附加到 v 中, v1: []
v.truncate(1);                  // 截断到指定长度，多余的元素被删除, v: [11]
v.retain(|x| *x > 10);          // 保留满足条件的元素，即删除不满足条件的元素

let mut v = vec![11, 22, 33, 44, 55];
// 删除指定范围的元素，同时获取被删除元素的迭代器, v: [11, 55], m: [22, 33, 44]
let mut m: Vec<_> = v.drain(1..=3).collect();    

let v2 = m.split_off(1);        // 指定索引处切分成两个 vec, m: [22], v2: [33, 44]
```

当然也可以像[数组切片](/basic/compound-type/array.html#数组切片)的方式获取 vec 的部分元素：
```rust
fn main() {
    let v = vec![11, 22, 33, 44, 55];
    let slice = &v[1..=3];
    assert_eq!(slice, &[22, 33, 44]);
}
```

更多技术细节，阅读 Vector 的[标准库文档](https://doc.rust-lang.org/std/vec/struct.Vec.html#)。

**数组排序**是重要且常用的知识，了解一下：
```rust
let mut vec = vec![1, 5, 10, 2, 15];    
vec.sort();    
print!("{:?}", vec);

// 如果 vector 内的数据类型无法直接比较，也可使用 sort_by 和闭包（后面会学到）来实现
// 比如我们计划把存放个人数据的 vector 按照年龄倒序排列：
// peoples.sort_by(|a, b| b.age.cmp(&a.age))	// 示例代码，无法运行, age：u32
```
非稳定排序 sort_unstable 和 sort_unstable_by 的说明，所谓的**非稳定**并不是指排序算法本身不稳定，而是指在排序过程中对相等元素的处理方式。在稳定 排序算法里，对相等的元素，不会对其进行重新排序。而在非稳定的算法里则不保证这点。

总体而言，非稳定排序的算法的速度会优于稳定排序算法，同时，稳定排序还会额外分配原数组一半的空间。

#### 延伸阅读

- [Rust 秘典 - Vector 动态数组的实现细节](https://doc.rust-lang.org/nomicon/vec/vec.html)
- [Vector 的内存布局说明](https://rust-book.junmajinlong.com/ch7/02_vec_capacity_reallocation.html)

### 2）字符串 str

> 字符串在 Rust 中反而是一个难点，要多花点心思理解透彻，人类语言本身亦很复杂。

在语言级别，只有字符串切片类型 `str` （切片类型本质上是一个胖指针），`&str` 表示为 str 类型的引用。

Rust 标准库提供的字符串类型：String、OsString、OsStr、CsString、CsStr 等。

字符串字面量（比如 "THE RUST LANGUAGE."，双引号包围），被编译器做了特殊处理，硬编码写入程序二进制文件中，并不是直接存到内存的堆（Heap）或者栈（Stack），程序加载时，放在单独的地方（内存静态数据区的全局字面量区）。

str 类型无法被修改，String 则是一个可增长、可改变且具有所有权的 UTF-8 编码字符串。

```rust
fn print_type_name<T>(_val: &T) {
    println!("{}", std::any::type_name::<T>());
}

fn main() {
	let lang = "THE RUST LANGUAGE."; // lang: &str，存放在栈上
	println!("{lang}");

	// "Let's Rusty!" 在程序加载期间被放入字面量内存区 Literals
	// String::from 从字面量内存区将其拷贝到内存堆(Heap)上, 
	// 然后将堆内存中该数据的地址保存到栈(Stack)内变量 hello 中
	let hello = String::from("Let's Rusty!"); // 等同 .to_string() 即 &str => String
	// &hello: String => &[String], &hello.as_str(): String => &str
	print_type_name(&hello);  // alloc::string::String
}
```

字符串 String 的底层的数据存储格式实际上是[u8]：一个字节数组，而不同语言的字符长度不一，通过索引的方式获取某个字符是不可预期的。

操作字符串:
```rust
let mut s = String::from("Hello ");

s.push_str("rust");			// 附加 &str	, 亦可以: s + "rust!";
s.push('!');	
s.insert(5, ',');				
s.insert_str(6, " i like");		// 插入到指定索引 &str
let mut s1 = s.replace("rust", "RUST");	// 替换 
s1.replace_range(7..8, "r");		// 替换指定范围，直接操作原字符串

println!("{s1}");

// 其他方法：remove、truncate、clear

// 遍历字符操作: 你好 👋
for c in "السلام عليكم".chars() { print!("{c} "); } // ا ل س ل ا م   ع ل ي ك م
for b in "Здравствуйте".bytes() { print!("{b} "); }
// 181 151 208 180 209 128 208 176 208 178 209 129 209 130 208 178 209 131 208 185 209 130 208 

```

关于字符串转义、Unicode 字符、UTF-8 的知识：

- https://crates.io/crates/utf8_slice
- https://doc.rust-lang.org/std/string/struct.String.html#utf-8


### 3）哈希 HashMap

---

更多[集合数据类型](https://doc.rust-lang.org/std/collections/index.html)，可以在后续需要时学习。


