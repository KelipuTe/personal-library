## 进程

- 2021-12-04

程序是静态的，进程是动态的，程序启动就变成进程，直到其运行结束。

### 跟踪命令行运行hello程序

命令行1负责运行，命令行自身的进程号可通过`echo $$`命令获取。

```
> ./hello
```

命令行2负责追踪，这里的72就是命令行1的进程id，追踪的内容输出到hello.log文件。

```
> strace -f -i -s 65535 -T -p 72 -o hello.log
strace: Process 72 attached
strace: Process 153 attached
^Cstrace: Process 72 detached
```

### hello.log文件内容

这里先看第一行：

- `72`表示跟踪的进程号
- `[00007f16e235a9a0]`表示系统函数的地址
- `read(0, ".", 1)`表示调用的系统函数
- `= 1`表示调用的系统函数的返回值
- `<4.166190>`表示调用的系统函数的执行时间

```
# 首先前面这一大段read和write，跟踪到的是命令行1的输入

72    [00007f16e235a9a0] read(0, ".", 1) = 1 <4.166190>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000330>
72    [00007f16e235aa00] write(2, ".", 1) = 1 <0.000315>
72    [00007f16e22a1630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000548>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000345>
72    [00007f16e235a9a0] read(0, "/", 1) = 1 <0.043209>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000191>
72    [00007f16e235aa00] write(2, "/", 1) = 1 <0.000239>
72    [00007f16e22a1630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000122>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000092>
72    [00007f16e235a9a0] read(0, "h", 1) = 1 <0.381996>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000532>
72    [00007f16e235aa00] write(2, "h", 1) = 1 <0.000546>
72    [00007f16e22a1630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000459>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000349>
72    [00007f16e235a9a0] read(0, "e", 1) = 1 <0.881355>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000477>
72    [00007f16e235aa00] write(2, "e", 1) = 1 <0.000353>
72    [00007f16e22a1630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000301>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000290>
72    [00007f16e235a9a0] read(0, "l", 1) = 1 <0.235798>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000251>
72    [00007f16e235aa00] write(2, "l", 1) = 1 <0.000363>
72    [00007f16e22a1630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000354>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000249>
72    [00007f16e235a9a0] read(0, "l", 1) = 1 <0.131463>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000204>
72    [00007f16e235aa00] write(2, "l", 1) = 1 <0.000293>
72    [00007f16e22a1630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000235>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000418>
72    [00007f16e235a9a0] read(0, "o", 1) = 1 <0.171883>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000294>
72    [00007f16e235aa00] write(2, "o", 1) = 1 <0.000496>
72    [00007f16e22a1630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000300>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000115>
72    [00007f16e235a9a0] read(0, "\r", 1) = 1 <0.649119>
72    [00007f16e235aa00] write(2, "\n", 1) = 1 <0.000206>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000201>
72    [00007f16e235f97c] ioctl(0, SNDCTL_TMR_STOP or TCSETSW, {B38400 opost isig icanon echo ...}) = 0 <0.000146>
72    [00007f16e22a1630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000184>
72    [00007f16e22a150d] rt_sigaction(SIGINT, {sa_handler=0x456bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000140>
72    [00007f16e22a150d] rt_sigaction(SIGTERM, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000162>
72    [00007f16e22a150d] rt_sigaction(SIGQUIT, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000061>
72    [00007f16e22a150d] rt_sigaction(SIGALRM, {sa_handler=0x456da0, sa_mask=[HUP INT ILL TRAP ABRT BUS FPE USR1 SEGV USR2 PIPE ALRM TERM XCPU XFSZ VTALRM SYS], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000049>
72    [00007f16e22a150d] rt_sigaction(SIGTSTP, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000047>
72    [00007f16e22a150d] rt_sigaction(SIGTTOU, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000047>
72    [00007f16e22a150d] rt_sigaction(SIGTTIN, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000040>
72    [00007f16e22a150d] rt_sigaction(SIGWINCH, {sa_handler=0x456740, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=0x49ba70, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000083>
72    [00007f16e236aae7] socket(AF_NETLINK, SOCK_RAW, NETLINK_AUDIT) = 3 <0.000040>
72    [00007f16e236a9d0] sendmsg(3, {msg_name={sa_family=AF_NETLINK, nl_pid=0, nl_groups=00000000}, msg_namelen=12, msg_iov=[{iov_base={len=24, type=0x464 /* NLMSG_??? */, flags=NLM_F_REQUEST, seq=0, pid=0}, iov_len=16}, {iov_base="\x2e\x2f\x68\x65\x6c\x6c\x6f\x00", iov_len=8}], msg_iovlen=2, msg_controllen=0, msg_flags=0}, 0) = 24 <0.000048>
72    [00007f16e235b050] close(3)       = 0 <0.000036>
72    [00007f16e22a150d] rt_sigaction(SIGINT, {sa_handler=0x456bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=0x456bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000071>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, [INT CHLD], [], 8) = 0 <0.000050>
72    [00007f16e235b137] pipe([3, 4])   = 0 <0.000095>

# clone的作用是创建一个进程

72    [00007f16e2330972] clone(child_stack=NULL, flags=CLONE_CHILD_CLEARTID|CLONE_CHILD_SETTID|SIGCHLD, child_tidptr=0x7f16e2c81a10) = 153 <0.000245>

# <unfinished ...> 表示产生了进程切换

153   [00007f16e22a1630] rt_sigprocmask(SIG_SETMASK, [],  <unfinished ...>
72    [00007f16e2331aa7] setpgid(153, 153 <unfinished ...>
153   [00007f16e22a1630] <... rt_sigprocmask resumed>NULL, 8) = 0 <0.000549>
72    [00007f16e2331aa7] <... setpgid resumed>) = 0 <0.000438>
153   [00007f16e22a150d] rt_sigaction(SIGTSTP, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400},  <unfinished ...>
72    [00007f16e22a1630] rt_sigprocmask(SIG_SETMASK, [],  <unfinished ...>
153   [00007f16e22a150d] <... rt_sigaction resumed>{sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000388>
72    [00007f16e22a1630] <... rt_sigprocmask resumed>NULL, 8) = 0 <0.000332>
153   [00007f16e22a150d] rt_sigaction(SIGTTIN, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000105>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, [CHLD],  <unfinished ...>
153   [00007f16e22a150d] rt_sigaction(SIGTTOU, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400},  <unfinished ...>
72    [00007f16e22a1630] <... rt_sigprocmask resumed>[], 8) = 0 <0.000331>
153   [00007f16e22a150d] <... rt_sigaction resumed>{sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000382>
153   [00007f16e2331aa7] setpgid(153, 153 <unfinished ...>
72    [00007f16e235b050] close(3 <unfinished ...>
153   [00007f16e2331aa7] <... setpgid resumed>) = 0 <0.000430>
153   [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, [CHLD TSTP TTIN TTOU],  <unfinished ...>
72    [00007f16e235b050] <... close resumed>) = 0 <0.000828>
153   [00007f16e22a1630] <... rt_sigprocmask resumed>[], 8) = 0 <0.000360>
72    [00007f16e235b050] close(4 <unfinished ...>
153   [00007f16e2360307] ioctl(255, TIOCSPGRP, [153] <unfinished ...>
72    [00007f16e235b050] <... close resumed>) = 0 <0.000391>
153   [00007f16e2360307] <... ioctl resumed>) = 0 <0.000325>
72    [00007f16e2360307] ioctl(255, TIOCGPGRP <unfinished ...>
153   [00007f16e22a1630] rt_sigprocmask(SIG_SETMASK, [],  <unfinished ...>
72    [00007f16e2360307] <... ioctl resumed>, [153]) = 0 <0.000402>
153   [00007f16e22a1630] <... rt_sigprocmask resumed>NULL, 8) = 0 <0.000394>
153   [00007f16e235b050] close(4 <unfinished ...>
72    [00007f16e22a1630] rt_sigprocmask(SIG_SETMASK, [],  <unfinished ...>
153   [00007f16e235b050] <... close resumed>) = 0 <0.000266>
72    [00007f16e22a1630] <... rt_sigprocmask resumed>NULL, 8) = 0 <0.000415>
153   [00007f16e235a9a0] read(3,  <unfinished ...>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, [CHLD],  <unfinished ...>
153   [00007f16e235a9a0] <... read resumed>"", 1) = 0 <0.000432>
72    [00007f16e22a1630] <... rt_sigprocmask resumed>[], 8) = 0 <0.000397>
153   [00007f16e235b050] close(3 <unfinished ...>

# 72进程阻塞，切换到153进程运行

72    [00007f16e233046c] wait4(-1,  <unfinished ...>
153   [00007f16e235b050] <... close resumed>) = 0 <0.000386>
153   [00007f16e22a150d] rt_sigaction(SIGHUP, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, NULL, 8) = 0 <0.000069>
153   [00007f16e22a150d] rt_sigaction(SIGILL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, NULL, 8) = 0 <0.000056>
153   [00007f16e22a150d] rt_sigaction(SIGTRAP, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, NULL, 8) = 0 <0.000114>
153   [00007f16e22a150d] rt_sigaction(SIGABRT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, NULL, 8) = 0 <0.000043>
153   [00007f16e22a150d] rt_sigaction(SIGFPE, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, NULL, 8) = 0 <0.000050>
153   [00007f16e22a150d] rt_sigaction(SIGBUS, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, NULL, 8) = 0 <0.000046>
153   [00007f16e22a150d] rt_sigaction(SIGSEGV, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, NULL, 8) = 0 <0.000046>
153   [00007f16e22a150d] rt_sigaction(SIGSYS, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, NULL, 8) = 0 <0.000046>
153   [00007f16e22a150d] rt_sigaction(SIGPIPE, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, NULL, 8) = 0 <0.000049>
153   [00007f16e22a150d] rt_sigaction(SIGALRM, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, NULL, 8) = 0 <0.000048>
153   [00007f16e22a150d] rt_sigaction(SIGXCPU, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, NULL, 8) = 0 <0.000055>
153   [00007f16e22a150d] rt_sigaction(SIGXFSZ, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, NULL, 8) = 0 <0.000049>
153   [00007f16e22a150d] rt_sigaction(SIGVTALRM, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, NULL, 8) = 0 <0.000064>
153   [00007f16e22a150d] rt_sigaction(SIGUSR1, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, NULL, 8) = 0 <0.000049>
153   [00007f16e22a150d] rt_sigaction(SIGUSR2, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, NULL, 8) = 0 <0.000054>
153   [00007f16e22a150d] rt_sigaction(SIGINT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=0x456bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000049>
153   [00007f16e22a150d] rt_sigaction(SIGQUIT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000068>
153   [00007f16e22a150d] rt_sigaction(SIGTERM, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000048>
153   [00007f16e22a150d] rt_sigaction(SIGCHLD, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f16e22a1400}, {sa_handler=0x4414a0, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000061>

# 153进程调用execve去执行hello程序了

153   [00007f16e2330c37] execve("./hello", ["./hello"], 0x879f60 /* 9 vars */) = 0 <0.001366>
153   [00007f0ab78b8aac] brk(NULL)      = 0xb17000 <0.000064>
153   [00007f0ab78b97da] mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f0ab7ac0000 <0.000065>
153   [00007f0ab78b96d7] access("/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory) <0.000069>
153   [00007f0ab78b9677] open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3 <0.000040>
153   [00007f0ab78b9604] fstat(3, {st_mode=S_IFREG|0644, st_size=13257, ...}) = 0 <0.000073>
153   [00007f0ab78b97da] mmap(NULL, 13257, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f0ab7abc000 <0.000037>
153   [00007f0ab78b9787] close(3)       = 0 <0.000037>

# 加载/lib64/libc.so.6

153   [00007f0ab78b9677] open("/lib64/libc.so.6", O_RDONLY|O_CLOEXEC) = 3 <0.000063>

# 文件头里的ELF表示这是个可执行文件

153   [00007f0ab78b9697] read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0`&\2\0\0\0\0\0@\0\0\0\0\0\0\0p\325 \0\0\0\0\0\0\0\0\0@\08\0\n\0@\0K\0J\0\6\0\0\0\5\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0000\2\0\0\0\0\0\0000\2\0\0\0\0\0\0\10\0\0\0\0\0\0\0\3\0\0\0\4\0\0\0`\354\30\0\0\0\0\0`\354\30\0\0\0\0\0`\354\30\0\0\0\0\0\34\0\0\0\0\0\0\0\34\0\0\0\0\0\0\0\20\0\0\0\0\0\0\0\1\0\0\0\5\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0203\34\0\0\0\0\0\0203\34\0\0\0\0\0\0\0 \0\0\0\0\0\1\0\0\0\6\0\0\0\3606\34\0\0\0\0\0\3606<\0\0\0\0\0\3606<\0\0\0\0\0\260Q\0\0\0\0\0\0\20\233\0\0\0\0\0\0\0\0 \0\0\0\0\0\2\0\0\0\6\0\0\0`k\34\0\0\0\0\0`k<\0\0\0\0\0`k<\0\0\0\0\0\360\1\0\0\0\0\0\0\360\1\0\0\0\0\0\0\10\0\0\0\0\0\0\0\4\0\0\0\4\0\0\0p\2\0\0\0\0\0\0p\2\0\0\0\0\0\0p\2\0\0\0\0\0\0D\0\0\0\0\0\0\0D\0\0\0\0\0\0\0\4\0\0\0\0\0\0\0\7\0\0\0\4\0\0\0\3606\34\0\0\0\0\0\3606<\0\0\0\0\0\3606<\0\0\0\0\0\20\0\0\0\0\0\0\0\240\0\0\0\0\0\0\0\20\0\0\0\0\0\0\0P\345td\4\0\0\0|\354\30\0\0\0\0\0|\354\30\0\0\0\0\0|\354\30\0\0\0\0\0\374j\0\0\0\0\0\0\374j\0\0\0\0\0\0\4\0\0\0\0\0\0\0Q\345td\6\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\20\0\0\0\0\0\0\0R\345td\4\0\0\0\3606\34\0\0\0\0\0\3606<\0\0\0\0\0\3606<\0\0\0\0\0\209\0\0\0\0\0\0\209\0\0\0\0\0\0\1\0\0\0\0\0\0\0\4\0\0\0\24\0\0\0\3\0\0\0GNU\0\215\251p}Q\216\210\27\233\33)\3\315\204l\202\20-\204\301\4\0\0\0\20\0\0\0\1\0\0\0GNU\0\0\0\0\0\2\0\0\0\6\0\0\0 \0\0\0\0\0\0\0\363\3\0\0\7\0\0\0\0\1\0\0\16\0\0\0\0000\20D\240 \2\1\210\3\346\220\305E\214\0\300\0\10\0\5\200\0`\300\200\0\r\212\f\0\4\20\0\210D2\10.@\210T<, \0162H&\204\300\214\4\10\0\2\2\16\241\254\32\4f\300\0\3002\0\300\0P\1 \201\10\204\v  ($\0\4 Z\0\20X\200\312DB(\0\6\200\20\30B\0 @\200\0\tP\0Q\212@\20\0\0\0\0\10\0\0\21\20", 832) = 832 <0.000039>
153   [00007f0ab78b9604] fstat(3, {st_mode=S_IFREG|0755, st_size=2156592, ...}) = 0 <0.000052>
153   [00007f0ab78b97da] mmap(NULL, 3985920, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f0ab74d2000 <0.000057>
153   [00007f0ab78b9877] mprotect(0x7f0ab7696000, 2093056, PROT_NONE) = 0 <0.000050>
153   [00007f0ab78b97da] mmap(0x7f0ab7895000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1c3000) = 0x7f0ab7895000 <0.000038>
153   [00007f0ab78b97da] mmap(0x7f0ab789b000, 16896, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f0ab789b000 <0.000073>
153   [00007f0ab78b9787] close(3)       = 0 <0.000041>
153   [00007f0ab78b97da] mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f0ab7abb000 <0.000032>
153   [00007f0ab78b97da] mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f0ab7ab9000 <0.000050>
153   [00007f0ab78a0efd] arch_prctl(ARCH_SET_FS, 0x7f0ab7ab9740) = 0 <0.000042>
153   [00007f0ab75621e0] access("/etc/sysconfig/strcasecmp-nonascii", F_OK) = -1 ENOENT (No such file or directory) <0.000038>
153   [00007f0ab75644e0] access("/etc/sysconfig/strcasecmp-nonascii", F_OK) = -1 ENOENT (No such file or directory) <0.000037>
153   [00007f0ab78b9877] mprotect(0x7f0ab7895000, 16384, PROT_READ) = 0 <0.000036>
153   [00007f0ab78b9877] mprotect(0x600000, 4096, PROT_READ) = 0 <0.000065>
153   [00007f0ab78b9877] mprotect(0x7f0ab7ac1000, 4096, PROT_READ) = 0 <0.000040>
153   [00007f0ab78b9857] munmap(0x7f0ab7abc000, 13257) = 0 <0.000060>
153   [00007f0ab75c14b4] fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(136, 2), ...}) = 0 <0.000048>
153   [00007f0ab75cafca] mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f0ab7abf000 <0.000077>

# 调用write函数输出hello world

153   [00007f0ab75c1ba0] write(1, "hello world\n", 12) = 12 <0.000066>
153   [00007f0ab7597da9] exit_group(0)  = ?

# 153进程退出

153   [????????????????] +++ exited with 0 +++

# 72进程知道153进程退出了，切换回72进程继续执行

72    [00007f16e233046c] <... wait4 resumed>[{WIFEXITED(s) && WEXITSTATUS(s) == 0}], WSTOPPED|WCONTINUED, NULL) = 153 <0.035081>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, [CHLD TSTP TTIN TTOU], [CHLD], 8) = 0 <0.000051>
72    [00007f16e2360307] ioctl(255, TIOCSPGRP, [72]) = 0 <0.000049>
72    [00007f16e22a1630] rt_sigprocmask(SIG_SETMASK, [CHLD], NULL, 8) = 0 <0.000044>
72    [00007f16e235fa69] ioctl(255, TCGETS, {B38400 opost isig icanon echo ...}) = 0 <0.000036>
72    [00007f16e2360307] ioctl(255, TIOCGWINSZ, {ws_row=30, ws_col=110, ws_xpixel=0, ws_ypixel=0}) = 0 <0.000049>
72    [00007f16e22a1630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000041>
72    [00007f16e22a1630] --- SIGCHLD {si_signo=SIGCHLD, si_code=CLD_EXITED, si_pid=153, si_uid=0, si_status=0, si_utime=0, si_stime=0} ---
72    [00007f16e233046c] wait4(-1, 0x7ffe15fab450, WNOHANG|WSTOPPED|WCONTINUED, NULL) = -1 ECHILD (No child processes) <0.000046>
72    [00007f16e22a1409] rt_sigreturn({mask=[]}) = 0 <0.000057>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000054>
72    [00007f16e22a150d] rt_sigaction(SIGINT, {sa_handler=0x456bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=0x456bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000053>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000032>
72    [00007f16e235aa00] write(1, "\33]0;@1fa648c62058:/code/demo/hello\7", 35) = 35 <0.000067>
72    [00007f16e235a2c5] stat("/var/mail/root", 0x7ffe15faacf0) = -1 ENOENT (No such file or directory) <0.000057>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, [CHLD TSTP TTIN TTOU], [], 8) = 0 <0.000034>
72    [00007f16e2360307] ioctl(255, TIOCSPGRP, [72]) = 0 <0.000055>
72    [00007f16e22a1630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000037>
72    [00007f16e22a150d] rt_sigaction(SIGINT, {sa_handler=0x456bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=0x456bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000053>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000031>
72    [00007f16e2360307] ioctl(0, TIOCGWINSZ, {ws_row=30, ws_col=110, ws_xpixel=0, ws_ypixel=0}) = 0 <0.000057>
72    [00007f16e2360307] ioctl(0, TIOCSWINSZ, {ws_row=30, ws_col=110, ws_xpixel=0, ws_ypixel=0}) = 0 <0.000090>
72    [00007f16e235fa69] ioctl(0, TCGETS, {B38400 opost isig icanon echo ...}) = 0 <0.000059>
72    [00007f16e235f97c] ioctl(0, SNDCTL_TMR_STOP or TCSETSW, {B38400 opost isig -icanon -echo ...}) = 0 <0.000037>
72    [00007f16e22a1630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000071>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, [INT QUIT ALRM TERM TSTP TTIN TTOU], [], 8) = 0 <0.000048>
72    [00007f16e22a150d] rt_sigaction(SIGINT, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=0x456bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000071>
72    [00007f16e22a150d] rt_sigaction(SIGTERM, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000067>
72    [00007f16e22a150d] rt_sigaction(SIGTERM, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000067>
72    [00007f16e22a150d] rt_sigaction(SIGQUIT, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000037>
72    [00007f16e22a150d] rt_sigaction(SIGQUIT, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000070>
72    [00007f16e22a150d] rt_sigaction(SIGALRM, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=0x456da0, sa_mask=[HUP INT ILL TRAP ABRT BUS FPE USR1 SEGV USR2 PIPE ALRM TERM XCPU XFSZ VTALRM SYS], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000091>
72    [00007f16e22a150d] rt_sigaction(SIGTSTP, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000047>
72    [00007f16e22a150d] rt_sigaction(SIGTSTP, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000049>
72    [00007f16e22a150d] rt_sigaction(SIGTTOU, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000058>
72    [00007f16e22a150d] rt_sigaction(SIGTTOU, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000049>
72    [00007f16e22a150d] rt_sigaction(SIGTTIN, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000055>
72    [00007f16e22a150d] rt_sigaction(SIGTTIN, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000046>
72    [00007f16e22a1630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000036>
72    [00007f16e22a150d] rt_sigaction(SIGWINCH, {sa_handler=0x49ba70, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f16e22a1400}, {sa_handler=0x456740, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f16e22a1400}, 8) = 0 <0.000036>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000048>
72    [00007f16e235aa00] write(2, "[root@1fa648c62058 hello]# ", 27) = 27 <0.000072>
72    [00007f16e22a1630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000058>
72    [00007f16e22a1630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000048>
72    [00007f16e235a9a0] read(0,  <detached ...>

```

### 进程的运行过程

72 是/bin/bash程序启动的，72是进程号。

```
72    [00007f16e2330972] clone(child_stack=NULL, flags=CLONE_CHILD_CLEARTID|CLONE_CHILD_SETTID|SIGCHLD, child_tidptr=0x7f16e2c81a10) = 153 <0.000245>
```

72进程调用clone或者fock系统函数，并执行得到一个新的子进程，这个新的子进程是153。

```
72    [00007f16e233046c] wait4(-1,  <unfinished ...>
```

72进程调用wait4在此阻塞。

```
153   [00007f16e2330c37] execve("./hello", ["./hello"], 0x879f60 /* 9 vars */) = 0 <0.001366>
```

153进程调用execve系统函数，执行elf可执行文件hello。`["./hello"]`是参数。`0x879f60 /* 9 vars */`是环境参数，在linux中环境参数是供所有应用程序使用的公共数据。

execve系统函数会加载elf文件的.text（程序指令）和.data（程序数据）到当前进程，并覆盖当前进程。

```
153   [00007f0ab78b9677] open("/lib64/libc.so.6", O_RDONLY|O_CLOEXEC) = 3 <0.000063>
```

加载程序所依赖的相关库文件`/lib64/libc.so.6`，它是共享目标文件，也叫共享库或动态库。它里面封装了一些系统函数。

这个`= 3`的3，是文件描述符，就是指当前进程在访问的文件，这个值一般大于等于0。

```
153   [00007f0ab78b9697] read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0`&\2\0\0\0\0\0@\0\0\0\0\0\0\0p\325 \0\0\0\0\0\0\0\0\0@\08\0\n\0@\0K\0J\0\6\0\0\0\5\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0000\2\0\0\0\0\0\0000\2\0\0\0\0\0\0\10\0\0\0\0\0\0\0\3\0\0\0\4\0\0\0`\354\30\0\0\0\0\0`\354\30\0\0\0\0\0`\354\30\0\0\0\0\0\34\0\0\0\0\0\0\0\34\0\0\0\0\0\0\0\20\0\0\0\0\0\0\0\1\0\0\0\5\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0203\34\0\0\0\0\0\0203\34\0\0\0\0\0\0\0 \0\0\0\0\0\1\0\0\0\6\0\0\0\3606\34\0\0\0\0\0\3606<\0\0\0\0\0\3606<\0\0\0\0\0\260Q\0\0\0\0\0\0\20\233\0\0\0\0\0\0\0\0 \0\0\0\0\0\2\0\0\0\6\0\0\0`k\34\0\0\0\0\0`k<\0\0\0\0\0`k<\0\0\0\0\0\360\1\0\0\0\0\0\0\360\1\0\0\0\0\0\0\10\0\0\0\0\0\0\0\4\0\0\0\4\0\0\0p\2\0\0\0\0\0\0p\2\0\0\0\0\0\0p\2\0\0\0\0\0\0D\0\0\0\0\0\0\0D\0\0\0\0\0\0\0\4\0\0\0\0\0\0\0\7\0\0\0\4\0\0\0\3606\34\0\0\0\0\0\3606<\0\0\0\0\0\3606<\0\0\0\0\0\20\0\0\0\0\0\0\0\240\0\0\0\0\0\0\0\20\0\0\0\0\0\0\0P\345td\4\0\0\0|\354\30\0\0\0\0\0|\354\30\0\0\0\0\0|\354\30\0\0\0\0\0\374j\0\0\0\0\0\0\374j\0\0\0\0\0\0\4\0\0\0\0\0\0\0Q\345td\6\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\20\0\0\0\0\0\0\0R\345td\4\0\0\0\3606\34\0\0\0\0\0\3606<\0\0\0\0\0\3606<\0\0\0\0\0\209\0\0\0\0\0\0\209\0\0\0\0\0\0\1\0\0\0\0\0\0\0\4\0\0\0\24\0\0\0\3\0\0\0GNU\0\215\251p}Q\216\210\27\233\33)\3\315\204l\202\20-\204\301\4\0\0\0\20\0\0\0\1\0\0\0GNU\0\0\0\0\0\2\0\0\0\6\0\0\0 \0\0\0\0\0\0\0\363\3\0\0\7\0\0\0\0\1\0\0\16\0\0\0\0000\20D\240 \2\1\210\3\346\220\305E\214\0\300\0\10\0\5\200\0`\300\200\0\r\212\f\0\4\20\0\210D2\10.@\210T<, \0162H&\204\300\214\4\10\0\2\2\16\241\254\32\4f\300\0\3002\0\300\0P\1 \201\10\204\v  ($\0\4 Z\0\20X\200\312DB(\0\6\200\20\30B\0 @\200\0\tP\0Q\212@\20\0\0\0\0\10\0\0\21\20", 832) = 832 <0.000039>
```

`/lib64/libc.so.6`是个elf可执行文件，可以看到文件前面的内容中的ELF字样。

```
153   [00007f0ab75c1ba0] write(1, "hello world\n", 12) = 12 <0.000066>
```

进程执行，调用系统函数write像屏幕输出hello world字符串。参数值12表示"hello world\n"有12个字节。

c语言的stdio.h头文件声明的printf函数的底层实现最终调用的就是系统函数write，而write的具体实现就在libc.so.6库里。

```
153   [00007f0ab7597da9] exit_group(0)  = ?
153   [????????????????] +++ exited with 0 +++
```

153进程调用系统函数exit_group(0)退出进程，0是进程退出状态码。

```
72    [00007f16e233046c] <... wait4 resumed>[{WIFEXITED(s) && WEXITSTATUS(s) == 0}], WSTOPPED|WCONTINUED, NULL) = 153 <0.035081>
```

父进程72，调用wait4函数回收退出的子进程，并回收子进程的内存资源。153就是退出的子进程的标识符。`WIFEXITED(s)`是个宏函数，`WEXITSTATUS(s)`拿到退出状态码。

至此整个hello程序执行结束。

### 运行时做了什么

### 运行时内存数据在哪里

### 内存数据有哪些

### 结束时做了什么

### 进程如何结束

- 进程调用exit_group(0)系统函数（程序里面的return 0）
- 进程调用exit系统函数退出进程
- 进程调用`_exit`或`_Exit`退出进程
- 进程接收到了中断信号
- 程序运行到最后一行代码