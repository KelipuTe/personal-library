## 中断信号

- 2021-12-12

### 硬件中断

中断请求，中断响应，中断返回，中断现场恢复。

主程序执行程序，这时产生中断请求（比如键盘按键），然后cpu会响应中断请求，响应结束之后返回，返回后要能恢复中断产生时的现场，然后继续执行程序。

### 软件中断

可以通过`man`命令（`man signal;man 7 signal`）查看linux中中断信号的相关文档。

#### 中断信号对进程的影响

- 进程终止
- 进程忽略
- 进程终止并产生core dump文件
- 进程停止
- 进程继续执行
- 执行进程信号对应的信号处理函数

#### 中断信号的相关动作

- 默认动作（default action，SIG_DFL）
- 忽略（ignore，SIG_IGN）
- 捕捉（signal hanlder）

#### 中断信号的产生

- 通过kill命令发送
- 在进程中使用kill函数
- 子进程退出时会产生中断信号
- 终端按下ctrl+c，产生SIGINT
- 使用alarm，raise等函数会产生中断信号
- 进程访问一些不存在的内存，或是非法内存，会产生SIGSEGV

#### 中断信号

在linux中，中断信号有64个，分位标准信号和实时信号。可以通过`kill -l`命令查看linux中的64个中断信号。其中，带RT（real time）的就是实时信号。

```
 1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL       5) SIGTRAP
 6) SIGABRT      7) SIGBUS       8) SIGFPE       9) SIGKILL     10) SIGUSR1
11) SIGSEGV     12) SIGUSR2     13) SIGPIPE     14) SIGALRM     15) SIGTERM
16) SIGSTKFLT   17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
21) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU     25) SIGXFSZ
26) SIGVTALRM   27) SIGPROF     28) SIGWINCH    29) SIGIO       30) SIGPWR
31) SIGSYS      34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
38) SIGRTMIN+4  39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
43) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
48) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
53) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
58) SIGRTMAX-6  59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
63) SIGRTMAX-1  64) SIGRTMAX
```

### 中断信号的使用

进程在收到中断信号后，如果编写了信号处理函数就执行信号处理函数，如果没有编写信号处理函数就会执行默认动作。

代码详见：`demo_c/demo_linux_c/signal/signal01.c`.

### 系统调用时中断

中断系统调用是指当前进程正在执行一些系统调用，此时进程收到了一个中断信号，此时该系统调用将会停止并返回错误，错误码（errno）一般是-1，错误（error）一般是EINTR。

进程正在执行系统调用，而且这个系统调用被阻塞。这时来了一个中断信号，结果会有两种。第一种，进程的信号处理函数已经执行返回，这时系统调用会重新开始。第二种，进程的信号处理函数已经执行返回，但是系统调用返回错误，错误码时EINTR。

代码详见：`demo_c/demo_linux_c/signal/sigaction.c`。

### 信号屏蔽字和未决集

Signal mask and pending signals

