## 进程结束

- 2021-12-07

### 代码和文件

- demo_c/demo/hello/hello.c
- demo_c/demo/hello/hello
- demo_c/demo/hello/hello.log

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

