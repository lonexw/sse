# 常用的集合 Collections

集合就像装东西的容器，经常会用到。

除了基本数据类型或固定大小的数组或元组存储特定的值，Rust 标准库还提供不少非常有用的集合数据结构来**存放动态数据**。

动态数据都在内存空间，存放在堆（Heap）上，在内存中彼此相邻排列。

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

> vector 动态数组还有很多[常用方法(标准库 API 文档)](https://doc.rust-lang.org/std/vec/struct.Vec.html)。

巧妙利用枚举来存放**不同数据类型**的值：
```rust
#[derive(Debug)]
enum TableCell {	// 定义一个枚举类型表示电子表格的数据
	Int(i32),		// 可能是数字
	Float(f64),		// 可能是浮点值
	Text(String),	// 可能是字符串
}

let row = vec![TableCell::Int(3), TableCell::Float(3.14), TableCell::Text("Rust".to_string())];

for cell in row { println!("Cell data is {:?}", cell); }
```

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

### 2）字符串 String

### 3）哈希 HashMap

---

更多[集合数据类型](https://doc.rust-lang.org/std/collections/index.html)，可以在后续需要时学习。


