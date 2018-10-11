# :whale: 进程, 线程, 协程, goroutine

## :fish: 进程 vs 线程

进程是运行态的程序， **是系统资源分配和调度的一个独立单位**。 不同进程之间通过 _进程间通信IPC)_ 来通信。

线程是 CPU 调度和分派的基本单位(注意进程是 **资源分配和调度** 的基本单位).

| | 进程 | 线程 |
|---|-----|-----|
|进程/线程空间内容 | 地址空间, 全局变量, 文件描述符, 子进程, 即将发生的报警, 信号和信号处理程序, 账户信号, 同步/互斥信号量 | 程序计数器 (pc), 寄存器, 堆栈, 状态 |
| 创建速度 | 慢 | 比进程快 10 ~ 100 倍 |
| 创建所需资源 | 多 | 少 |
| 上下文切换 | 慢 | 快 |
| 独立内存空间 | 有 | 无 |
| 通信/同步方式 | IPC | 共享数据, 互斥锁, 信号量 |

## :fish: 协程

**协程是一种用户态的轻量级线程**, 协程的调度完全由用户控制. 协程之间是合作的关系, 这也意味着, 协程之间不需要互斥锁, 信号量等同步原语, 也不需要任何系统级别的同步支持. 协程实现也不需要任何的系统调用或者任何阻塞. 协程之间是并发的, 而不是同步的, 这一点和线程不同. 控制权在多个协程之间转换. 每个协程可以维持自己的每次调用的之间的状态, 而子程序调用不会保持每次调用之间的状态.

协程实现生产者和消费者的简单举例:

    var q := new queue
    coroutine produce
        loop
            while q is not full
                create some new items
                add the items to q
            yield to consume

    coroutine consume
        loop
            while q is not empty
                remove some items from q
                use the items
            yield to produce

由上面的例子可见, 生产者和消费者之间是合作的关系, 而不是竞态的关系, 通过 `yield` 交出控制权给另一个协程.

## :fish: goroutine

goroutine 不是协程, 官方的说法是: 'A goroutine is a lightweight thread managed by the Go runtime'. **是 Go 运行时管理的轻量级的线程**. Go 实现了 goroutine 的调度器, goroutine 的上下文只有 2k, 而线程的上下文需要几兆的空间. goroutine 的上下文切换开销是很低的.

所以 go 语言级别支持高并发.

## 参考资料

- [Coroutine, 维基百科](https://en.wikipedia.org/wiki/Coroutine)
