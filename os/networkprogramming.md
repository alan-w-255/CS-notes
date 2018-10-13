# :whale: 网络编程

<!-- TOC -->

- [:whale: 网络编程](#whale-网络编程)
  - [:fish: TCP VS UDP](#fish-tcp-vs-udp)
    - [:bee: TCP 编程](#bee-tcp-编程)
    - [:bee: UDP 编程](#bee-udp-编程)
    - [:bee: TCP 粘包](#bee-tcp-粘包)

<!-- /TOC -->

## :fish: TCP VS UDP
  
| TCP          | UDP         |
| ------------ | ----------- |
| 面向连接     | 无连接      |
| 面向字节流   | 面向报文    |
| 首部 20 字节 | 首部 8 字节 |
| 保证可靠     | 不保证可靠  |

TCP 充分实现了数据传输时各种控制功能:

- 丢包的重发控制
- 对分包的顺序控制

这些 UDP 都没有. TCP 通过 **校验和**, **序列号**, **确认应答**, **重发控制**, **连接管理**, **窗口控制**等机制实现可靠性

### :bee: TCP 编程

:sunflower: 服务器端:

1. :herb: 创建一个 socket, `socket()`
2. :herb: 设置 socket 属性, `setsockopt()`
3. :herb: 绑定 IP 地址, 端口, `bind()`
4. :herb: 开启监听, `listen()`
5. :herb: 接受连接, `accept()`
6. :herb: 收发数据, `send()` 和 `recv()`, 或者 `read()` 和 `write()`
7. :herb: 关闭网络连接
8. :herb: 关闭监听

<details><summary> c 示例 </summary>
<p>

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>

int main (int argc, char *argv[]) {
  int server_sockfd; //服务器端套接字
  int client_sockfd; // 客户端套接字
  int len;
  struct sockaddr_in my_addr; // 服务器网络地址结构体
  struct sockaddr_in remote_addr; // 客户端网络地址结构体
  int sin_size;
  char buf[BUFSIZ]; // 数据缓冲区
  memset(&my_addr, 0, sizeof(my_addr)); // 数据初始化 -- 清零
  my_addr.sin_family = AF_INET; // 设置为 IP 通信
  my_addr.sin_addr.s_addr = INADDR_ANY; // 服务器 IP 地址 -- 允许连接到所有本地地址上.
  my_addr.sin_port = htons(8000); // 服务器端口

  if ((server_sockfd=socket(PF_INET, SOCK_STREAM, 0)) < 0) {
    perror("socket error");
    return 1;
  }

  if(bind(server_sockfd,(struct sockaddr *)&my_addr,sizeof(struct sockaddr))<0)
  {
    perror("bind error");
    return 1;
  }

  /*监听连接请求--监听队列长度为5*/
  if(listen(server_sockfd,5)<0)
  {
    perror("listen error");
    return 1;
  };

  sin_size=sizeof(struct sockaddr_in);

  /*等待客户端连接请求到达*/
  if((client_sockfd=accept(server_sockfd,(struct sockaddr *)&remote_addr,&sin_size))<0)
  {
    perror("accept error");
    return 1;
  }
  printf("accept client %s/n",inet_ntoa(remote_addr.sin_addr));
  len=send(client_sockfd,"Welcome to my server/n",21,0);//发送欢迎信息

  /*接收客户端的数据并将其发送给客户端--recv返回接收到的字节数，send返回发送的字节数*/
  while((len=recv(client_sockfd,buf,BUFSIZ,0))>0))
  {
    buf[len]='/0';
    printf("%s/n",buf);
    if(send(client_sockfd,buf,len,0)<0)
    {
      perror("write error");
      return 1;
    }
  }


  /*关闭套接字*/
  close(client_sockfd);
  close(server_sockfd);

  return 0;
}
```

</p></details>

:sunflower: client 端

1. :herb: 创建一个 socket, `socket()`
2. :herb: 连接, `connect()`
3. :herb: 收发数据, `write()` 或 `send()`, `read()` 或 `recv()`
4. :herb: 关闭连接, `close()`

<details><summary> c 示例 </summary>
<p>

```c
#include <arpa/inet.h>
#include <netinet/in.h>
#include <stdio.h>
#include <sys/socket.h>
#include <sys/types.h>

int main(int argc, char *argv[]) {
  int client_sockfd;
  int len;
  struct sockaddr_in remote_addr;  //服务器端网络地址结构体
  char buf[BUFSIZ];                //数据传送的缓冲区
  memset(&remote_addr, 0, sizeof(remote_addr));  //数据初始化--清零
  remote_addr.sin_family = AF_INET;              //设置为IP通信
  remote_addr.sin_addr.s_addr = inet_addr("127.0.0.1");  //服务器IP地址
  remote_addr.sin_port = htons(8000);                    //服务器端口号

  /*创建客户端套接字--IPv4协议，面向连接通信，TCP协议*/
  if ((client_sockfd = socket(PF_INET, SOCK_STREAM, 0)) < 0) {
    perror("socket error");
    return 1;
  }

  /*将套接字绑定到服务器的网络地址上*/
  if (connect(client_sockfd, (struct sockaddr *)&remote_addr,
              sizeof(struct sockaddr)) < 0) {
    perror("connect error");
    return 1;
  }
  printf("connected to server/n");
  len = recv(client_sockfd, buf, BUFSIZ, 0);  //接收服务器端信息
  buf[len] = '/0';
  printf("%s", buf);  //打印服务器端信息

  /*循环的发送接收信息并打印接收信息（可以按需发送）--recv返回接收到的字节数，send返回发送的字节数*/
  while (1) {
    printf("Enter string to send:");
    scanf("%s", buf);
    if(!strcmp(buf,"quit") break;
    len=send(client_sockfd,buf,strlen(buf),0);
    len=recv(client_sockfd,buf,BUFSIZ,0);
    buf[len]='/0';
    printf("received:%s/n",buf);
  }

  /*关闭套接字*/
  close(client_sockfd);

  return 0;
}
```

</p></details>

### :bee: UDP 编程

### :bee: TCP 粘包

