## 进程间通信

2021-12-13

---

### 进程间通信

IPC（Interprocess Communication，进程间通信）指的是在同一台机器上的不同进程之间传播或交换信息。

ipc的方法主要包括pipe（管道）和ipc对象（消息队列、信号量、共享内存）。管道包括匿名管道和命名管道。ipc对象有两个标准，一个是比较老的"SYSTEM V"标准的消息队列、信号量、共享内存，另一个是POSIX标准的消息队列、信号量、共享内存。

### 管道

管道的实现依赖linux内核。

#### 匿名管道

可以通过在linux系统中使用`man 2 pipe`命令查看相关文档或者访问在线文档："pipe(2)"（https://man7.org/linux/man-pages/man2/pipe.2.html）。

示例代码：`demo_c/demo_linux_c/ipc/pipe10.c`

#### 命名管道

可以通过在linux系统中使用`man 7 pipe`命令查看相关文档或者访问在线文档："pipe(7)"（https://man7.org/linux/man-pages/man7/pipe.7.html）。

命名管道可以是父子进程，也可以是非父子进程。

示例代码：

- `demo_c/demo_linux_c/ipc/pipe20.c`
- `demo_c/demo_linux_c/ipc/pipe21_w.c`
- `demo_c/demo_linux_c/ipc/pipe21_r.c`

### ipc对象

可以通过在linux系统中使用`man ipc`命令查看相关文档或者访问在线文档："ipc(2)"（https://man7.org/linux/man-pages/man2/ipc.2.html）。

ipc对象包括消息队列、信号量、共享内存。创建ipc对象时需要指定一个key（键，标识符）。key是指ipc对象在操作系统内部的唯一标识，在多个进程通信时用作关联。

ipc相关linux命令：`ipcs`、`ipcrm`。

#### 消息队列

消息队列的实现依赖linux内核，并且它的数据存储在内核中。

消息队列相关的文档有`msgget(2)`、`msgctl(2)`、`msgsnd(2)`、`msgrcv(2)`，分别是消息队列创建、消息队列控制、发送消息、接收消息。

可以通过在linux系统中使用`man 2 msgget`、`man 2 msgctl`、`man 2 msgsnd`、`man 2 msgrcv`命令查看相关文档或者访问在线文档：

- "msgget(2)"（https://man7.org/linux/man-pages/man2/msgget.2.html）
- "msgctl(2)"（https://man7.org/linux/man-pages/man2/msgctl.2.html）
- "msgop(2)"（https://man7.org/linux/man-pages/man2/msgsnd.2.html）
- "msgop(2)"（https://man7.org/linux/man-pages/man2/msgrcv.2.html）

示例代码：

- `demo_c/demo_linux_c/ipc/mq10_get.c`
- `demo_c/demo_linux_c/ipc/mq10_ctl.c`
- `demo_c/demo_linux_c/ipc/mq10_snd.c`
- `demo_c/demo_linux_c/ipc/mq10_rcv.c`

消息队列创建完成后，可以通过`ipcs`命令查看，通过输出信息的key字段就可以找到创建的消息队列。

#### 信号量

信号量本质上是个计数器，linux内核有相应的数据结构来维护。

一般最常用的信号量是二值信号量（0和1），用于进程间同步或是互斥操作。

信号量相关的文档有`sem_overview(7)`、`semget(2)`、`semctl(2)`、`semop(2)`，分别是信号量概述、信号量创建、信号量控制、信号量操作。

可以通过在linux系统中使用`man 7 sem_overview`、`man 2 semget`、`man 2 semctl`、`man 2 semop`命令查看相关文档或者访问在线文档：

- "sem_overview(7)"（https://man7.org/linux/man-pages/man7/sem_overview.7.html）
- "semget(2)"（https://man7.org/linux/man-pages/man2/semget.2.html）
- "semctl(2)"（https://man7.org/linux/man-pages/man2/semctl.2.html）
- "semop(2)"（https://man7.org/linux/man-pages/man2/semop.2.html）

示例代码：

- `demo_c/demo_linux_c/ipc/sem10_get.c`
- `demo_c/demo_linux_c/ipc/sem10_ctl.c`
- `demo_c/demo_linux_c/ipc/sem10_op.c`

二值信号量主要特点就是集合里只有一个信号量，并且信号量的值只有0和1这两个。

对信号量进行加1叫做V，对它进行减1操作叫做P，和操作系统信号量的pv操作是一样的。

二值信号量主要用于进程间同步，保证数据的一致性，对关键核心代码做临界，控制共享内存。

#### 共享内存

申请一块内存，进程间通过关联这块内存从而达到进程间通信的目的，它是IPC进程间通信中最快的。

信号量相关的文档有`shmget(2)`、`shmop(2)`，分别是共享内存创建、共享内存连接和断开。

可以通过在linux系统中使用`man 2 shmget`、`man 2 shmat`、`man 2 shmdt`命令查看相关文档或者访问在线文档：

- "shmget(2)"（https://man7.org/linux/man-pages/man2/shmget.2.html）
- "shmop(2)"（https://man7.org/linux/man-pages/man2/shmat.2.html）
- "shmop(2)"（https://man7.org/linux/man-pages/man2/shmdt.2.html）

示例代码：

- `demo_c/demo_linux_c/ipc/shm10_get.c`
- `demo_c/demo_linux_c/ipc/shm10_at.c`
- `demo_c/demo_linux_c/ipc/shm10_dt.c`