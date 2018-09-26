# IPC

## 管道

通常指无名管道, 是 UNIX 系统 IPC 最古老的形式.

- 半双工(数据只能在一个方向上流动), 具有固定的读端和写端
- 只能在父子或者兄弟进程之间通信
- 可以看作一个特殊的文件, 只能存在于内存中

原型:

```c
#include<unistd.h>
int pipe(int fd[2]); // return 1 if success, -1 if error
```

pipe 会创建两个文件描述符. fd[0] 为读端, fd[1] 为写端.

example

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

## FIFO

也叫命名管道, 它是一种文件类型.

- 命名管道可以在无关的进程之间交换数据, 与无名管道不同
- 命名管道有路径名与之相关联, 它存在于文件系统中

原型

```c
#include <sys/stat.h>
int mkfifo(const char *pathname, mode_t mode);
```

## 信号量, 共享内存, 消息队列

信号量 (semaphore) 是一个计数器. 信号量用于实现进程间的互斥与同步. 而不是用于存储进程间通信数据.

共享内存 (Shared Memory), 指两个或多个进程共享一个给定的存储区.

- 共享内存是最快的一种 IPC, 因为进程是直接对内存进行存取
- 因为多个进程可以同时操作, 所以需要进行同步
- 信号量+共享内存通常结合在一起使用, 信号量用来同步对共享内存的访问

原型

```c
#include <sys/shm.h>
int shmget(key_t key, size_t size, int flag);
void *shmat(int shm_id, const void *addr, int flag);
int shmdt(void *addr);
```

## 套接字