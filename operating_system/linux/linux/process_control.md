## 进程控制

- 2021-12-07

### 代码和文件

- demo_c/demo_linux_c/fork/fork.c
- demo_c/demo_linux_c/fork/fork
- demo_c/demo_linux_c/execv/execv01.c
- demo_c/demo_linux_c/execv/execv01
- demo_c/demo_linux_c/execv/execv02.c
- demo_c/demo_linux_c/execv/execv02
- demo_c/demo_linux_c/nice/nice.c
- demo_c/demo_linux_c/nice/nice

### fork-and-exec

### fork

系统函数fork可以创建一个进程（子进程）。系统函数fork通过复制调用进程创建一个新进程，调用进程作为父进程，新进程作为子进程。

两个进程的内存地址有同样的内容（程序数据和程序指令），父进程和子进程代码是一样的，子进程会从fork的下一行代码继续执行。一般父进程先执行，除非被阻塞。

但是两个进程运行在不同的内存空间里面，进程间是隔离的。两个进程执行内存写操作（定义新的变量并赋值，修改变量的值，定义新的函数）或者文件映射（进程间通信）时互不影响。

#### 父进程和子进程的区别

- 子进程有自己独立的唯一的进程标识（pid）
- 子进程的父进程id和父进程的pid是一样的
- 子进程不会继承父进程的内存锁

#### 系统函数fock的返回值

新进程创建成功时，父进程拿到子进程的pid，子进程拿到0。可以根据返回值判断那个是父进程。

新进程创建失败时，父进程拿到-1，子进程不会被创建。errno变量会存储错误信息。

#### copy-on-write（写时复制）

在fork之后exec之前，两个进程用的是相同的物理空间，子进程的代码段、数据段、堆栈都是指向父进程的物理空间。

也就是说，两者的虚拟空间不同，但其对应的物理空间是同一个。当父子进程中有更改相应段的行为发生时，再为子进程相应的段分配物理空间。

如果不是因为exec，内核会给子进程的数据段、堆栈段分配相应的物理空间（至此两者有各自的进程空间，互不影响）。

而代码段继续共享父进程的物理空间（两者的代码完全相同）。而如果是因为exec，由于两者执行的代码不同，子进程的代码段也会分配单独的物理空间。

#### 进程标识pid和ppid

getpid()返回调用进程的pid，getppid()返回调用进程的父进程pid。

在编码测试时需要注意，必须先让子进程先执行，父进程后执行，打印出来的ppid才是正确的。

如果父进程在子进程执行前先跑完了，子进程的ppid就会变成1，这时子进程就变成孤儿进程。1就是系统进程，被1号进程接管有可能就变成后台进程了。

#### vfork

系统方法vfork创建子进程并阻塞父进程，直到子进程退出。而且vfork和fork不一样，vfork创建出来的子进程和父进程共享内存。

vfork这个函数有bug，当代码`return 0`结束或者执行到左后一行结束时，有可能会报`Segmentation fault (core dumped)`错误。但是使用exit(0)或者_exit(0)的时候不会。

通过strace命令追踪可以发现报错时，子进程调用系统函数exit_group(0)退出，但是父进程没有调用系统函数exit_group(0)。不报错时，两个进程都调用系统函数exit_group(0)退出。

### exec

#### execve

在程序里可以使用的和系统函数execve作用相同的函数有好几个，这里用execv举例。代码详见`demo_c/demo_linux_c/execv/execv01.c`和`demo_c/demo_linux_c/execv/execv02.c`。

#### 进程的执行顺序

进程的执行（调度）顺序受到PRI（priority）和NI（nice）值控制，这两个值越小，进程优先级越高。这两个值可以通过`ps -ely`命令查看PRI和NI，也可以通过`top`命令查看PR和NI。

可以使用`nice`和`renice`命令调整进程的优先级。nice的值的范围是-20~19。`nice`用于进程启动之前（`nice - run a program with modified scheduling priority`）。`renice`用于进程启动之后（`renice - alter priority of running processes`）。

在代码中，有getpriority函数可以查看进程优先级，有setpriority函数和nice函数可以调整进程优先级。getpriority和setpriority这两个函数的定义是这样的：

```c
#include <sys/time.h>
#include <sys/resource.h>
​int getpriority(int which, id_t who);
int setpriority(int which, id_t who, int prio);

// The value which is one of PRIO_PROCESS, PRIO_PGRP, or PRIO_USER, and who is interpreted relative to which (a process identi‐ fier for PRIO_PROCESS, process group identifier for PRIO_PGRP, and a user ID for PRIO_USER). A zero value for who denotes (respectively) the calling process, the process group of the calling process, or the real user ID of the calling process.
```

这里需要注意的是，which参数选什么，who就要对应的填什么。代码详见`demo_c/demo_linux_c/nice/nice.c`。
