# 异步编程：无畏并发

### 并发 vs 并行

![f37dd89173715d0e21546ea171c8a915_1440w.png](https://pic1.zhimg.com/80/f37dd89173715d0e21546ea171c8a915_1440w.png)

**并发和并行都是对“多任务”处理的描述，其中并发是轮流处理，而并行是同时处理**。

并发的关键在于：**快速轮换处理不同的任务**，给用户带来所有任务同时在运行的假象。

线程局部变量

[GitHub - Amanieu/thread_local-rs: Per-object thread-local storage for Rust](https://github.com/Amanieu/thread_local-rs)

导致一些问题：

- 竞态条件(race conditions)，多个线程以非一致性的顺序同时访问数据资源
- 死锁(deadlocks)，两个线程都想使用某个资源，但是又都在等待对方释放资源后才能使用，结果最终都无法继续执行
- 一些因为多线程导致的很隐晦的 BUG，难以复现和解决

### 并发模型

Rust 在设计时考虑的权衡就是运行时(Runtime)。出于 Rust 的系统级使用场景，且要保证调用 C 时的极致性能，它最终选择了尽量小的运行时实现。

线程（Thread）的概念，绿色线程 M:N 模型、需要较大的运行时来管理线程。

- 当多个线程以不一致的顺序访问数据或资源时产生的竞争状态（race condition）。
- 当两个线程同时尝试获取对方持有的资源时产生的死锁（deadlock），它会导致这两个线程无法继续运行。

spawn 创建线程，返回一个自持有所有权的 **JoinHandle**，调用它的 join 方法可以**阻塞当前线程**直到对应的新线程运行结束。

与 move 关键字配合使用，它允许你在*某个线程中使用来自另一个线程的数据，跨线程的传递某些值的所有权*。

### 消息传递并发机制 Message Passing

线程或 actor 之间通过给彼此发送包含数据的消息来进行通信。

编程中的通道由发送者（transmitter）和接收者（receiver）两个部分组成。

当你丢弃了发送者或接收者的任何一端时，我们就称相应的通道被关闭（closed）了。

**mpsc::channel** 函数创建了一个新的通道（Multiple producer，single consumer）

tx.send(val).unwrap(); send函数会获取参数的所有权，并在参数传递时将所有权转移给接收者。

recv和try_recv。我们使用的recv（也就是receive的缩写）会阻塞主线程的执行直到有值被传入通道。一旦有值被传入通道，recv就会将它包裹在Result<T, E>中返回。而如果通道的发送端全部关闭了，recv则会返回一个错误来表明当前通道再也没有可接收的值。

try_recv方法不会阻塞线程，它会立即返回Result<T, E>：当通道中存在消息时，返回包含该消息的Ok变体；否则便返回Err变体。

mpsc :: Sender :: clone(&tx) 克隆生产者

### 共享内存的并发机制

多个线程可以同时访问相同的内存地址。

#### 互斥体（mutex：Mutual Exclusion）

一次只允许一个线程访问数据。为了访问互斥体中的数据，线程必须首先发出信号来获取互斥体的锁（lock）。锁是互斥体的一部分，这种数据结构被用来记录当前谁拥有数据的唯一访问权。通过锁机制，互斥体守护（guarding）了它所持有的数据。

• 必须在使用数据前尝试获取锁。

• 必须在使用完互斥体守护的数据后释放锁，这样其他线程才能继续完成获取锁的操作。

调用它的lock方法来获取锁。这个调用会阻塞当前线程直到我们取得锁为止。

一旦获取了锁，我们便可以将它的返回值num视作一个指向内部数据的可变引用。Mutex<T>是一种智能指针。更准确地说，**对lock的调用会返回一个名为MutexGuard的智能指针。**

- 通过实现Deref来指向存储在内部的数据
- 通过实现Drop来完成自己离开作用域时的自动解锁操作

原子引用计数 **Arc<T>, 需要付出一定的性能开销才能实现线程安全。**

> 使用场景：可以将计算分割为多个独立的部分，并将它们分配至不同的线程中，然后使用Mutex<T>来允许不同的线程更新计算结果中与自己有关的那一部分。

### Sync trait and Send trait

std::marker 作为标签trait，Send与Sync甚至没有任何可供实现的方法。它们仅仅被用来强化与并发相关的不可变性。

只有实现了**::Send::** trait的类型才可**以安全地在线程间转移所有权**。除了Rc<T>等极少数的类型，几乎所有的Rust类型都实现了Send trait.

任何完全由Send类型组成的复合类型都会被自动标记为Send.

只有实现了**::Sync::** trait的类型才可以**安全地被多个线程引用.**

## [用条件变量(Condvar)控制线程的同步](https://course.rs/advance/concurrency-with-threads/sync1.html#%E7%94%A8%E6%9D%A1%E4%BB%B6%E5%8F%98%E9%87%8Fcondvar%E6%8E%A7%E5%88%B6%E7%BA%BF%E7%A8%8B%E7%9A%84%E5%90%8C%E6%AD%A5)

### Thread Pool：改进服务器吞吐量

一块预先分配的线程，用于等待并随时处理可能的任务。

控制线程池数量，避免 DoS 攻击。

fork/join 模型、单线程异步 I/O 模型

### Tokio `M:N` 模型

[理解tokio的核心(2): task - Rust入门秘籍](https://rust-book.junmajinlong.com/ch100/02_understand_tokio_task.html)

[](https://rustcc.cn/article?id=8af74de6-1e3d-4596-94ca-c3da45509f58)

# Tokio

[Setup | Tokio - An asynchronous Rust runtime](https://tokio.rs/tokio/tutorial/setup)

[tokio 概览 - Rust语言圣经(Rust Course)](https://course.rs/async-rust/tokio/overview.html)

[Tutorial | Tokio - An asynchronous Rust runtime](https://tokio.rs/tokio/tutorial)




