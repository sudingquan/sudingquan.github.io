---
title: linux中IO多路复用的三种方式select、poll、epoll
date: 2019-07-09 14:56:21
tags: linux
categories: linux
---

> I/O多路复用的本质是通过一种机制（系统内核缓冲I/O数据），让单个进程可以监视多个文件描述符，一旦某个描述符就绪（一般是读就绪或写就绪），能够通知程序进行相应的读写操作

## IO多路复用

select、poll 和 epoll 都是 Linux 提供的 IO 多路复用方式。

与多进程和多线程相比，IO多路复用的最大优点是系统开销小，系统不必创建进程/线程，也不必维护这些进程/线程，从而大大减小了系统的开销。

## Select

首先分析一下`select`函数

```c
int select(int nfds, fd_set *readfds, fd_set *writefds,fd_set *exceptfds, struct timeval *timeout);
```

### 参数说明

**int nfds** 的值为所有文件描述符中的最大值加1。

**fd_set \*readfds , fd_set \*writefds , fd_set \*exceptfds**
可以理解成一个集合，其中的元素是文件描述符。fd_set类型变量每一位代表了一个描述符。我们也可以认为它只是一个由很多二进制位构成的数组。中间的三个参数指定我们要让内核测试读、写和异常条件的文件描述符集合。如果对某一个的条件不感兴趣，就可以把它设为`NULL`。

**struct timeval \*timeout** 指定了`select`等待文件描述符做好准备的时间。

### 返回值

返回`fd_set`集合中就绪的读、写，或出错的文件描述符数量，返回0表示超时，返回-1表示出错；

<!-- more -->

### Select运行机制

`select`的机制中提供一种`fd_set`的数据结构，实际上是一个long类型的数组，每一个数组元素都能与一打开的文件描述符建立联系，当调用`select`时，由内核根据IO状态修改fd_set的内容，由此来通知执行了`select`的进程哪一文件描述符变为了就绪状态。

### Select的问题

1. 我们从`select`那里仅仅知道了，有I/O事件发生了，但却并不知道是哪个元素，我们只能无差别遍历集合中所有的元素，这样的话时间复杂度为O(n)，而且如果同时监视的元素越多，这个时间越长。
2. 为了减少数据拷贝带来的性能损坏，内核对被监控的`fd_set`集合大小做了限制，并且这个是通过宏控制的，大小不可改变(限制为1024)。

## Poll

poll的机制与select类似，与select在本质上没有多大差别，管理多个描述符也是进行轮询，根据描述符的状态进行处理，但是poll没有最大文件描述符数量的限制。也就是说，poll解决了上述的问题2，但是没有解决性能开销较大的问题

`poll`函数原型如下

```c
int poll(struct pollfd *fds, nfds_t nfds, int timeout);

struct pollfd {
  int   fd;         /* file descriptor */
  short events;     /* requested events */
  short revents;    /* returned events */
};
```

`poll`改变了文件描述符集合的存储方式，使用了`pollfd`结构而不是`select`的`fd_set`结构，使得`poll`支持的文件描述符集合远大于`select`的1024。

### 参数说明

**struct pollfd \*fds** `fds`是一个`struct pollfd`类型的数组，用于存放需要检测其状态的socket描述符，一个`pollfd`结构体表示一个被监视的文件描述符其中。

**nfds_t nfds** 记录数组`fds`中描述符的总数量

**int timeout**类似`select`的`struct timeval`，为超时时间。

### 返回值

类似于`select`函数，返回`fds`集合中就绪的读、写，或出错的文件描述符数量，返回0表示超时，返回-1表示出错；

## Epoll

epoll在Linux2.6内核正式提出，是基于事件驱动的I/O方式，相对于select来说，epoll没有描述符个数限制，使用一个文件描述符管理多个描述符，将用户关心的文件描述符的事件存放到内核的一个事件表中，这样在用户空间和内核空间的copy只需一次。

linux提供的epoll相关的函数如下：

```c
int epoll_create(int size);
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);
int epoll_wait(int epfd, struct epoll_event * events, int maxevents, int timeout);
```

1. **epoll_create** 函数创建一个epoll描述符，参数`size`表明要监听的描述符数量。调用成功时返回一个epoll描述符，失败时返回-1。

   注意：使用完epoll后，必须调用close()关闭，否则可能导致fd被耗尽。

2. **epoll_ctl** 函数注册要监听的事件类型。四个参数解释如下：

   - `epfd` 表示epoll描述符。
   - `op`表示fd操作类型，有如下3种：
     - `EPOLL_CTL_ADD`添加新的fd到epfd中
     - `EPOLL_CTL_MOD`修改已存在的fd的监听事件
     - `EPOLL_CTL_DEL`从epfd中删除一个fd
   - `fd` 是要监听的描述符。
   - `event` 表示要监听的事件。

   epoll_event 结构体定义如下：

   ```c
   struct epoll_event {
       __uint32_t events;  /* Epoll events */
       epoll_data_t data;  /* User data variable */
   };
   
   typedef union epoll_data {
       void *ptr;
       int fd;
       __uint32_t u32;
       __uint64_t u64;
   } epoll_data_t;
   ```

   `events`可以是以下几个宏的集合：

   `EPOLLIN`：表示对应的文件描述符可以读（包括对端SOCKET正常关闭）；
   `EPOLLOUT`：表示对应的文件描述符可以写；
   `EPOLLPRI`：表示对应的文件描述符有紧急的数据可读（这里应该表示有带外数据到来）；
   `EPOLLERR`：表示对应的文件描述符发生错误；
   `EPOLLHUP`：表示对应的文件描述符被挂断；
   `EPOLLET`： 将EPOLL设为边缘触发(Edge Triggered)模式，这是相对于水平触发(Level Triggered)来说的。
   `EPOLLONESHOT`：只监听一次事件，当监听完这次事件之后，如果还需要继续监听这个socket的话，需要再次把这个socket加入到EPOLL队列里

3. **epoll_wait** 函数等待事件的就绪，成功时返回就绪的事件数目，调用失败时返回 -1，等待超时返回 0。

   - `epfd` 是epoll描述符
   - `events` 表示从内核得到的就绪事件集合
   - `maxevents` events的大小
   - `timeout` 表示等待的超时事件

### Epoll实现

```c
#define PORT 8888
#define MAX_EVENTS 10

int creat_listen_socket() {
    int listen_socket;
    if ((listen_socket = socket(AF_INET, SOCK_STREAM, 0)) < 0) {
        perror("socket() error");
        return -1;
    }
    struct sockaddr_in my_addr;
    memset(&my_addr, 0, sizeof(my_addr));
    my_addr.sin_family = AF_INET;
    my_addr.sin_port = htons(PORT);
    my_addr.sin_addr.s_addr = htonl(INADDR_ANY);
    if (bind(listen_socket, (struct sockaddr *)&my_addr, sizeof(my_addr)) < 0) {
        perror("bind");
        return -1;
    }
    if (listen(listen_socket, 5) < 0) {
        perror("listen");
    }
    return listen_socket;
}

int main() {
    struct epoll_event ev, events[MAX_EVENTS];
    int listen_sock, conn_sock, nfds, epollfd;
    /* Code to set up listening socket, 'listen_sock',
       (socket(), bind(), listen()) omitted */
    listen_sock = creat_listen_socket();
    epollfd = epoll_create1(0);
    if (epollfd == -1) {
        perror("epoll_create1");
        exit(EXIT_FAILURE);
    }

    ev.events = EPOLLIN;
    ev.data.fd = listen_sock;
    if (epoll_ctl(epollfd, EPOLL_CTL_ADD, listen_sock, &ev) == -1) {
        perror("epoll_ctl: listen_sock");
        exit(EXIT_FAILURE);
    }

    for (;;) {
        nfds = epoll_wait(epollfd, events, MAX_EVENTS, -1);
        if (nfds == -1) {
            perror("epoll_wait");
            exit(EXIT_FAILURE);
        }
        struct sockaddr_in client_addr;
        unsigned int addrlen = sizeof(client_addr);
        for (int n = 0; n < nfds; ++n) {
            if (events[n].data.fd == listen_sock) {
                conn_sock = accept(listen_sock, (struct sockaddr *) &client_addr, &addrlen);
                if (conn_sock == -1) {
                    perror("accept");
                    exit(EXIT_FAILURE);
                }
                int imode = 1;
                ioctl(conn_sock, FIONBIO, &imode); //将新连接设置成非阻塞状态
                ev.events = EPOLLIN | EPOLLET; //将新连接加入epoll监听
                ev.data.fd = conn_sock;
                if (epoll_ctl(epollfd, EPOLL_CTL_ADD, conn_sock, &ev) == -1) {
                    perror("epoll_ctl: conn_sock");
                    exit(EXIT_FAILURE);
                }
            } else {
                do_use_fd(events[n].data.fd);
            }
        }
    }
    return 0;
}
```

## 总结

|            |                       select                       |                       poll                       |                            Epoll                             |
| :--------: | :------------------------------------------------: | :----------------------------------------------: | :----------------------------------------------------------: |
|  操作方式  |                        遍历                        |                       遍历                       |                             回调                             |
|  底层实现  |                        数组                        |                       链表                       |                            哈希表                            |
|   IO效率   |      每次调用都进行线性遍历，时间复杂度为O(n)      |     每次调用都进行线性遍历，时间复杂度为O(n)     | 事件通知方式，每当fd就绪，系统注册的回调函数就会被调用，将就绪fd放到readyList里面，时间复杂度O(1) |
| 最大连接数 |              1024（x86）或2048（x64）              |                      无上限                      |                            无上限                            |
|   fd拷贝   | 每次调用select，都需要把fd集合从用户态拷贝到内核态 | 每次调用poll，都需要把fd集合从用户态拷贝到内核态 |  调用epoll_ctl时拷贝进内核并保存，之后每次epoll_wait不拷贝   |

------

*既然select，poll，epoll都是I/O多路复用的具体的实现，之所以现在同时存在，其实他们也是不同历史时期的产物*

- *select出现是1984年在BSD里面实现的*。
- *14年之后也就是1997年才实现了poll，其实拖那么久也不是效率问题， 而是那个时代的硬件实在太弱，一台服务器处理1千多个链接简直就是神一样的存在了，select很长段时间已经满足需求*。
- *2002, 大神 Davide Libenzi 实现了epoll*。