# :whale: IPC

## :fish: 管道 (无名管道) vs FIFO (有名管道)

管道通常指无名管道, 是 UNIX 系统 IPC 最古老的形式.

FIFO 也叫命名管道, 它是一种文件类型, 存在于文件系统中.

| 无名管道                               | 有名管道     |
| -------------------------------------- | ------------ |
| **半双工** 只能在一个方向上流动        | 全双工     |
| 只能用于父母兄弟, 有共同祖先的进程之间 | 任意进程之间 |
| 只能存在于内存空间, 不存在于文件系统   |  存在于文件系统中, 不会随进程终结而删除 |
| 有缓冲区           |  有缓冲区      |
| 读写不保证原子性 |  读写不保证原子性 |


管道原型:

```c
#include<unistd.h>
int pipe(int fd[2]); // return 1 if success, -1 if error
```

pipe 会创建两个文件描述符. fd[0] 为读端, fd[1] 为写端.

<details><summary> c 语言使用管道示例 </summary>
<p>

```c
#include<stdio.h>
#include<unistd.h>

int main() {
  int fd[2];
  pid_t pid;
  char buff[20];
  if(pipe(fd) < 0) printf("create Pipe Error\n")
  if ((pid = fork()) < 0) {
    close(fd[0]); // 关闭读端
    write(fd[1], "hello world\n", 12);
  } else {
    close(fd[1]);// 关闭写端
    read(fd[0], buff, 20);
    printf("%s", buff);
  }
  return 0;
}
```

</p></details>

FIFO 原型

```c
#include <sys/stat.h>
int mkfifo(const char *pathname, mode_t mode);
```

## :fish: 信号量, 共享内存, 消息队列

信号量 (semaphore) 是一个计数器. 信号量用于实现进程间的互斥与同步. 而不是用于存储进程间通信数据.

共享内存 (Shared Memory), 指两个或多个进程共享一个给定的存储区.

- :bee: 共享内存是最快的一种 IPC, 因为进程是直接对内存进行存取
- :bee: 因为多个进程可以同时操作, 所以需要进行同步
- :bee: 信号量+共享内存通常结合在一起使用, 信号量用来同步对共享内存的访问

原型

```c
#include <sys/shm.h>
// 创建或获取一个共享内存: 成功返回内存ID, 失败返回 -1
int shmget(key_t key, size_t size, int flag);
// 连接共享内存到当前进程的地址空间: 成功返回指向共享内存的指针, 失败返回 -1
void *shmat(int shm_id, const void *addr, int flag);
int shmdt(void *addr);
int shmctl(int shm_id, int cmd, struct shmid_ds *buf);
```

## :fish: 套接字

可用于不同机器之间的进程通信.