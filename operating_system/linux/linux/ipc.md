## 进程间通信

### 进程间通信

IPC（Interprocess Communication，进程间通信）指的是在同一台机器上的不同进程之间传播或交换信息。

ipc的方法主要包括pipe（管道）和ipc对象（消息队列、信号量、共享内存）。管道包括匿名管道和命名管道。

ipc对象有两个标准，一个是比较老的"SYSTEM V"标准的消息队列、信号量、共享内存，另一个是POSIX标准的消息队列、信号量、共享内存。

### 管道

详见linux文档：`pipe(7) - overview of pipes and FIFOs`

管道的实现依赖linux内核。

#### 匿名管道

示例代码：`demo_c/demo_linux_c/ipc/anonymous_pipe/anonymous.c`

#### 命名管道

在win10环境的docker环境得linux环境中创建fifo文件时会出现`Input/output error`报错。这是因为docker目录是win10目录映射来的，创建不了fifo文件。解决办法是用linux环境中的目录，测试的时候可以先用`/tmp`目录。

命名管道可以是父子进程，也可以是非父子进程。

示例代码：

- 父子进程：`demo_c/demo_linux_c/ipc/named_pipes/parent_child.c`
- 非父子进程读取：`demo_c/demo_linux_c/ipc/named_pipes/pipe_read.c`
- 非父子进程写入：`demo_c/demo_linux_c/ipc/named_pipes/pipe_write.c`

### ipc对象

详见linux文档：`ipc(2) - System V IPC system calls`。

ipc对象包括消息队列、信号量、共享内存。创建ipc对象时需要指定一个key（键，标识符）。key是指ipc对象在操作系统内部的唯一标识，在多个进程通信时用作关联。

ipc相关的linux命令：`ipcs`、`ipcrm`。

#### 消息队列

详见linux文档：`mq_overview(7) - overview of POSIX message queues`。

消息队列的实现依赖linux内核，并且它的数据存储在内核中。

示例代码：

- 创建：`demo_c/demo_linux_c/ipc/message_queue/msgget.c`
- 控制：`demo_c/demo_linux_c/ipc/message_queue/msgctl.c`
- 发送：`demo_c/demo_linux_c/ipc/message_queue/msgsnd.c`
- 接收：`demo_c/demo_linux_c/ipc/message_queue/msgrcv.c`

消息队列创建完成后，可以通过`ipcs`命令查看，通过key字段可以找到创建的消息队列。

向消息队列发送消息后，used-bytes字段会显示消息队列使用的大小，messages字段会显示未处理消息的条数。

#### 信号量

详见linux文档：`sem_overview(7) - overview of POSIX semaphores`。

信号量本质上是个计数器，linux内核有相应的数据结构来维护。

示例代码：

- 创建：`demo_c/demo_linux_c/ipc/semaphores/semget.c`
- 控制：`demo_c/demo_linux_c/ipc/semaphores/semctl.c`
- 操作：`demo_c/demo_linux_c/ipc/semaphores/semop.c`

一般最常用的信号量是二值信号量（0和1），用于进程间同步或是互斥操作。二值信号量主要特点就是集合里只有一个信号量，并且信号量的值只有0和1这两个。对信号量进行加1叫做V，对它进行减1操作叫做P，和操作系统信号量的pv操作是一样的。二值信号量主要用于进程间同步，保证数据的一致性，对关键核心代码做临界，控制共享内存。

示例代码：`demo_c/demo_linux_c/ipc/semaphores/passeren_vrijgeven.c`

#### 共享内存

详见linux文档：`shm_overview(7) - overview of POSIX shared memory`。

申请一块内存，进程间通过关联这块内存从而达到进程间通信的目的，它是IPC进程间通信中最快的。

示例代码：

- 创建：`demo_c/demo_linux_c/ipc/shared_memory/shmget.c`
- 连接和写入：`demo_c/demo_linux_c/ipc/shared_memory/shmat.c`
- 读取和断开：`demo_c/demo_linux_c/ipc/shared_memory/shmdt.c`
