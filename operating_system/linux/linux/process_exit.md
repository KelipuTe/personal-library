## 进程结束

- 2021-12-07

### 代码和文件

- demo_c/demo_linux_c/hello/hello.c
- demo_c/demo_linux_c/hello/hello
- demo_c/demo_linux_c/hello/hello.log
- demo_c/demo_linux_c/wait/wait.c
- demo_c/demo_linux_c/wait/wait

### 进程退出的方式

- 程序运行到最后一行代码
- 进程调用系统函数`exit_group(0)`
- 进程调用`exit(0)`函数退出进程
- 进程调用`_exit(0)`函数或`_Exit(0)`函数退出进程
- 进程接收到了中断信号

调用abort()函数，进程会被异常终止。

### 不同退出方式的区别

程序运行到最后一行代码，代码`return 0`和调用`exit(0)`函数退出进程时，会输出输出缓冲区内的内容。而调用`_exit(0)`函数或`_Exit(0)`函数退出进程时不会。也就是对于`printf("hello")`来说，前面3个方式会输出hello，后面两个不会输出。

### 退出状态码

当进程结束时，会返回一个退出状态码，一般都是0，0表示成功退出。

```
[pid   204] [00007f6e21067da9] exit_group(300) = ?
[pid   204] [????????????????] +++ exited with 44 +++
```

退出状态码最大1个字节（也就是最大只能是255），大于1个字节会被处理（和255逻辑与）。比如返回300，最后会被处理成44。

使用`echo $?`命令可以打印上一个程序的退出状态码。

```
[00007f67fa7f7630] --- SIGCHLD {si_signo=SIGCHLD, si_code=CLD_EXITED, si_pid=204, si_uid=0, si_status=44, si_utime=0, si_stime=0} ---
[00007f67fa88646c] wait4(-1, 0x7ffdc7754a50, WNOHANG|WSTOPPED|WCONTINUED, NULL) = -1 ECHILD (No child processes)
```

子进程退出时会向父进程发送SIGCHLD中断信号，同时父进程调用系统函数wait4也会返回并且得到退出的相关信息

### 进程回收

当子进程调用exit(0)退出，但是父进程没有运行结束而且没有回收子进程的时候，子进程会变成僵尸进程。当父进程也结束的时候，操作系统会回收僵尸进程和父进程。

当进程变成僵尸进程时，它的内存数据还驻留在内存中，同时在`/proc`目录还有相关文件没有移除掉，还在占用系统资源。如果僵尸进程过多，会导致系统资源紧张，会影响操作系统的运行。所以我们必须要回收退出的子进程。

可以使用系统函数wait()或waitpid()回收子进程。系统函数wait()和或waitpid()会阻塞父进程，直到有子进程退出。

### 示例

`demo_c/demo_linux_c/wait/wait.c`，通过系统函数wait回收子进程。

```
child,pid=107
child exit,pid=107,exit status=0
parent,process,pid=107
parent,process,pid=107
parent,process,pid=107
...
```

`demo_c/demo_linux_c/waitpid/waitpid01.c`，通过系统函数waitpid回收正常退出的子进程。

```
child,pid=109
child exit,pid=109,exit status=0
child terminated normally,exit status=0
parent,process,pid=109
parent,process,pid=109
parent,process,pid=109
...
```

`demo_c/demo_linux_c/waitpid/waitpid02.c`，通过系统函数waitpid回收被信号终止的子进程。程序运行后，在另外一个终端上，通过`kill -s SIGUSR1 111`向子进程111发送SIGUSR1信号。

```
child,pid=111
child,pid=111
child,pid=111
child exit,pid=111,exit status=10
child terminated by a signal,exit status=10
parent,process,pid=111
parent,process,pid=111
...
```

`demo_c/demo_linux_c/waitpid/waitpid03.c`，测试信号处理函数。程序运行后，在另外一个终端上。

先通过`kill -s SIGSTOP 119`向子进程119发送SIGSTOP信号，这时进程会停止作业（并没有退出）。因为进程已经停止了，所以并不会输出信号的值。

再通过`kill -s SIGCONT 119`向子进程119发送SIGCONT信号，这时进程会恢复作业，并输出`signal no=18`，也就是收到的SIGCONT信号的值。

```
child,pid=119
child,pid=119
child,pid=119
signal no=18
child,pid=119
child,pid=119
child,pid=119
...
```

通过strace命令检测，可以得到下面的内容。进程确实收到了SIGSTOP信号和SIGCONT信号。

```
--- SIGSTOP {si_signo=SIGSTOP, si_code=SI_USER, si_pid=84, si_uid=0} ---
--- stopped by SIGSTOP ---
--- SIGCONT {si_signo=SIGCONT, si_code=SI_USER, si_pid=84, si_uid=0} ---
write(1, "signal no=18\r\n", 14)        = 14
```
