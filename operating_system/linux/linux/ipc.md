## 进程间通信

2021-12-13

---

### 进程间通信

IPC（Interprocess Communication，进程间通信）指的是在同一台机器上的不同进程之间传播或交换信息。

ipc的方法主要包括pipe（管道）和ipc对象（消息队列、信号量、共享内存）。管道包括匿名管道和命名管道。ipc对象有两个标准，一个是比较老的"SYSTEM V"标准的消息队列、信号量、共享内存，另一个是POSIX标准的消息队列、信号量、共享内存。

### 管道

管道的实现依赖linux内核。

#### 匿名管道

可以通过在linux系统中使用`man 2 pipe`命令或者访问在线文档"pipe(2)"（https://man7.org/linux/man-pages/man2/pipe.2.html）查看相关文档。

示例代码：`demo_c/demo_linux_c/ipc/pipe10.c`

#### 命名管道

可以通过在linux系统中使用`man 7 pipe`命令或者访问在线文档"pipe(7)"（https://man7.org/linux/man-pages/man7/pipe.7.html）查看相关文档。

命名管道可以是父子进程，也可以是非父子进程。

示例代码：`demo_c/demo_linux_c/ipc/pipe20.c`、`demo_c/demo_linux_c/ipc/pipe21_w.c`、`demo_c/demo_linux_c/ipc/pipe21_r.c`

### ipc对象

可以通过在linux系统中使用`man ipc`命令或者访问在线文档"ipc(2)"（https://man7.org/linux/man-pages/man2/ipc.2.html）查看相关文档。

ipc对象包括消息队列、信号量、共享内存。创建ipc对象时需要指定一个key（键，标识符）。key是指ipc对象在操作系统内部的唯一标识，在多个进程通信时用作关联。

ipc相关linux命令：`ipcs`、`ipcrm`。

#### 消息队列

消息队列的实现依赖linux内核，并且它的数据存储在内核中。

消息队列相关的文档有msgctl(2)、msgget(2)、msgrcv(2)、msgsnd(2)，分别是创建消息队列，接收消息，发送消息。

可以通过在linux系统中使用`man 2 msgctl`、`man 2 msgget`、`man 2 msgrcv`、`man 2 msgrcv`命令或者访问在线文档"msgctl(2)"（https://man7.org/linux/man-pages/man2/msgctl.2.html）"、"msgget(2)"（https://man7.org/linux/man-pages/man2/msgget.2.html）"、msgop(2)"（https://man7.org/linux/man-pages/man2/msgrcv.2.html）"、msgop(2)"（https://man7.org/linux/man-pages/man2/msgsnd.2.html）查看相关文档。

示例代码：`demo_c/demo_linux_c/ipc/pipe20.c`、`demo_c/demo_linux_c/ipc/pipe21_w.c`、`demo_c/demo_linux_c/ipc/pipe21_r.c`

消息队列创建完成后，可以通过`ipcs -l`命令查看，通过输出信息的key字段就可以找到创建的消息队列。