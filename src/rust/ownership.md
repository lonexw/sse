# Understanding Ownership 所有权系统

> ::A way to manage memory,::  you need fighting with the **borrow checker.**

图书的抽象表达。

### Stack and Heap

[理解Rust内存管理 - Rust入门秘籍](https://rust-book.junmajinlong.com/ch5/00.html)

[20 张图揭开「内存管理」的迷雾，瞬间豁然开朗](https://zhuanlan.zhihu.com/p/152119007)

### Ownership Rules

1. Each value in Rust has a variable that's called  its owner.
2. There can only be one owner at a time.
3. When the owner goes out of scope, the value will be dropped.

String and slice

基本概念介绍：https://www.youtube.com/watch?v=TJTDTyNdJdY&list=PLZaoyhMXgBzozt1LeHkCv8ERUPxhXT1eE


### 借用（borrowing）


**获取变量的引用，**常规引用是一个指针类型，指向了对象存储的内存地址。

*T 解引用，解出引用所指向的值。

**同一作用域，特定数据只能有一个可变引用。**

变量在离开作用域后，就自动释放其占用的内存。其实，在 C++ 中，也有这种概念: *Resource Acquisition Is Initialization (RAII)*。如果你使用过 RAII 模式的话应该对 Rust 的 `drop` 函数并不陌生。

![v2-8cc4ed8cd06d60f974d06ca2199b8df5_1440w.png](https://pic3.zhimg.com/80/v2-8cc4ed8cd06d60f974d06ca2199b8df5_1440w.png)

**把结构体中具有所有权的字段转移出去后，将无法再访问该字段，但是可以正常访问其它的字段**。

https://www.youtube.com/watch?v=1QoT9fmPYr8

## The Smart Pointer

https://www.youtube.com/watch?v=CTTiaOo4cbY&list=PLZaoyhMXgBzozt1LeHkCv8ERUPxhXT1eE&index=2

指针：a reference，指代包含内存地址的变量，这个地址被用于索引。

- & 符号，引用，Rust 中常见的一种指针
- * 符号，解引用行为，跟踪引用并跳转到它指向的值。
- 智能指针（smart pointer）则是一些数据结构，它们的行为类似于指针但拥有**额外的元数据和附加功能**
   - Struct + Deref + Drop
   - ::Deref trait:: 使得智能指针结构体的实例拥有与引用一致的行为
   - ::Drop trait:: 则使你可以自定义智能指针离开作用域时运行的代码
- String 和 Vec<T> 都可以被算作智能指针类型。
- Rc （Reference Counting）：引用计数智能指针类型。
   - Rc<T>类型的实例会在内部维护一个用于记录值引用次数的计数器，从而确认这个值是否仍在使用
   - Rc<T>的Drop实现会在Rc<T>离开作用域时自动将引用计数减1。
   - Rc<T>通过不可变引用使你可以在程序的不同部分之间共享只读数据。
   - 单线程场景使用
- Box<T> : 用于在堆上（Heap）分配值，栈中保留一个指向堆数据的指针。
   - 当你拥有一个无法在编译时确定大小的类型，但又想要在一个要求固定尺寸的上下文环境中使用这个类型的值时。
   - 当你需要传递大量数据的所有权，但又不希望产生大量数据的复制行为时
   - 当你希望拥有一个实现了指定trait的类型值，但又不关心具体的类型时。
- Ref<T> 和 RefMut<T> 可以通过RefCell<T>访问，是一种可以**在运行时**而不是编译时执行借用规则的类型。

> 引用是只借用数据的指针；而与之相反地，大多数智能指针本身就拥有（所有权）它们指向的数据。

### 解引用转换（Deref coercion）

Rust通过实现解引用转换功能，使程序员在调用函数或方法时无须多次显式地使用&和*运算符来进行引用和解引用操作。

• 当T: Deref<Target=U>时，允许&T转换为&U。

• 当T: DerefMut<Target=U>时，允许&mut T转换为&mut U。

• 当T: Deref<Target=U>时，允许&mut T转换为&U。

### 引用计数

调用Rc::clone来增加Rc<T>实例的strong_count引用计数。

使用Rc<T>的引用来调用Rc::downgrade函数会返回一个类型为Weak<T>的智能指针，这一操作会让Rc<T>中weak_count的计数增加1，而不会改变strong_count的状态。**Rc<T>并不会在执行清理操作前要求weak_count必须减为0。**

> 强引用可以被我们用来共享一个Rc实例的所有权，而弱引用则不会表达所有权关系。一旦强引用计数减为0，任何由弱引用组成的循环就会被打破。因此，弱引用不会造成循环引用。

### 内部可变性（Interior Mutability）

允许你在只持有不可变引用的前提下对数据进行修改 unsafe。对于使用一般引用和Box<T>的代码，Rust会在编译阶段强制代码遵守这些借用规则。而对于**::使用RefCell<T>的代码，Rust则只会在运行时检查这些规则::**，并在出现违反借用规则的情况下触发panic来提前中止程序。

运行时的借用规则检查同样能够帮助我们避免数据竞争。

> RefCell<T>类型代表了其持有数据的**唯一**所有权

在创建不可变和可变引用时分别使用语法&与&mut。对于RefCell<T>而言，我们需要使用**borrow与borrow_mut**方法来实现类似的功能。**borrow方法和borrow_mut方法会分别返回Ref<T>与RefMut<T>这两种智能指针**，由于这两种智能指针都实现了Deref，所以我们可以把它们当作一般的引用来对待。

RefCell<T>会记录当前存在多少个活跃的Ref<T>和RefMut<T>智能指针。每次调用borrow方法时，RefCell<T>会将活跃的不可变借用计数加1，并且在任何一个Ref<T>的值离开作用域被释放时，不可变借用计数将减1。RefCell<T>会基于这一技术来维护和编译器同样的借用检查规则：在任何一个给定的时间里，它**只允许你拥有多个不可变借用或一个可变借用**。

#### ::Rc<T> + RefCell<T>::

Rc<T>允许多个所有者持有同一数据，但只能提供针对数据的不可变访问。如果我们在Rc<T>内存储了RefCell<T>，那么就可以定义出**拥有多个所有者且能够进行修改的值**了。

#### 其他类型

Cell<T>选择了通过复制来访问数据

[Introduction - The Rustonomicon](https://doc.rust-lang.org/nomicon/index.html)



