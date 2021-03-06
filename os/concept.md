# :whale: 概念

<!-- TOC -->

- [:whale: 概念](#whale-概念)
  - [:fish: OS 基本功能](#fish-os-基本功能)
    - [:honeybee: 进程管理](#honeybee-进程管理)
    - [:honeybee: 内存管理](#honeybee-内存管理)
    - [:honeybee: 文件管理](#honeybee-文件管理)
    - [:honeybee: 设备管理](#honeybee-设备管理)
  - [:fish: 系统调用](#fish-系统调用)
  - [:fish: 并发 vs 并行](#fish-并发-vs-并行)
  - [:fish: 虚拟](#fish-虚拟)
  - [:fish: 异步, 同步, 阻塞, 非阻塞](#fish-异步-同步-阻塞-非阻塞)
  - [:fish: 字节序](#fish-字节序)

<!-- /TOC -->

## :fish: OS 基本功能

### :honeybee: 进程管理

- 进程控制
- 进程同步
- 进程通信
- 死锁处理
- 处理机调度

### :honeybee: 内存管理

- 内存分配
- 地址映射
- 内存保护与共享
- 虚拟内存

### :honeybee: 文件管理

- 文件存储空间管理
- 目录管理
- 文件读写管理与保护

### :honeybee: 设备管理

- 缓冲管理
- 设备分配
- 设备处理
- 虚拟设备

## :fish: 系统调用

如果一个进程在用户态需要使用内核态的功能, 就进行系统调用从而陷入内核, 由操作系统代为完成.

Linux 主要的系统调用

| Task     | Commands                    |
| -------- | --------------------------- |
| 进程控制 | fork(); exit(); wait();     |
| 进程通信 | pipe(); shmget(); mmap();   |
| 文件操作 | open(); read(); write();    |
| 设备操作 | ioctl(); read(); write();   |
| 信息维护 | getpid(); alarm(); sleep(); |
| 安全     | chmod(); umask(); chow();   |

## :fish: 并发 vs 并行

并发是指在宏观上一段时间内能够同时运行多个程序, 而并行则是指同一时刻能运行多个指令.

并行需要硬件支持, 如多流水或者多处理器. 在多核处理器上, 多线程之间可以并行运行.

## :fish: 虚拟

虚拟技术把一个物理实体转换为多个逻辑实体.
主要有两种虚拟技术: 时分复用技术和空分复用.

时分复用: 多个进程在同一个处理器上并发执行. 每个进程轮流占有处理器, 每次只执行一个小个时间片并快速切换.

虚拟内存使用了空分复用技术，它将物理内存抽象为地址空间，每个进程都有各自的地址空间。地址空间和物理内存使用页进行交换，地址空间的页并不需要全部在物理内存中，当使用到一个没有在物理内存的页时，执行页面置换算法，将该页置换到内存中。

## :fish: 异步, 同步, 阻塞, 非阻塞

- 同步和异步是针对 **被调用方** 而言的.

异步是指等被调用方被调用后立即返回, 但是没有调用结果, 当等到结果后通过信息通知调用方.
同步是指被调用方等到有计算结果后才返回.

- 而阻塞和非阻塞是针对 **调用方** 而言的.

调用方等待调用返回 -- 阻塞.
调用方不等待调用返回 -- 非阻塞

## :fish: 字节序

:herb: 小端字节序: 高位放在高地址, 低位放在低地址.

:herb: 大端字节序: 高位放在低地址, 低位放在高地址.

计算机内部处理都是小端字节序, 网络传输和文件存储都是大端字节序.

> 只有读取的时候, 才必须区分字节序, 其它情况都不用考虑