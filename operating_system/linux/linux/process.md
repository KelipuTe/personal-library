## 进程

- 2021-12-04

### 进程是什么

程序启动就变成进程，直到其运行结束。程序是静态的，进程是动态的。

### 进程的观察方式

- ps命令
- top命令
- pstree命令
- strace命令

### 代码和文件

- demo_c/demo_linux_c/hello/hello.c
- demo_c/demo_linux_c/hello/hello
- demo_c/demo_linux_c/write/write.c
- demo_c/demo_linux_c/write/write

### hello

这里使用一个终端1运行，使用另一个终端2监控终端1。终端1可通过`echo $$`命令获取自身的进程号。终端2通过strace命令和终端1的进程号监控终端1。

```shell
strace -f -i -s 65535 -T -p 1 -o hello.log
```

这里的1就是终端2的进程号，追踪到的内容输出到hello.log文件。

```
strace: Process 1 attached
strace: Process 79 attached
^Cstrace: Process 1 detached
```

终端1运行hello程序，终端2在输出前两行之后，按ctrl+c终止监视，会输出第三行。

### hello.log

追踪到的内容如下：

```
1     [00007f67fa8b09a0] read(0, ".", 1) = 1 <1.955794>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000311>
1     [00007f67fa8b0a00] write(2, ".", 1) = 1 <0.000459>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000352>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000450>
1     [00007f67fa8b09a0] read(0, "/", 1) = 1 <0.042319>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000302>
1     [00007f67fa8b0a00] write(2, "/", 1) = 1 <0.000283>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000283>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000254>
1     [00007f67fa8b09a0] read(0, "h", 1) = 1 <0.351256>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000344>
1     [00007f67fa8b0a00] write(2, "h", 1) = 1 <0.000543>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000289>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000172>
1     [00007f67fa8b09a0] read(0, "e", 1) = 1 <0.149488>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000084>
1     [00007f67fa8b0a00] write(2, "e", 1) = 1 <0.000296>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000174>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000253>
1     [00007f67fa8b09a0] read(0, "l", 1) = 1 <0.220069>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000223>
1     [00007f67fa8b0a00] write(2, "l", 1) = 1 <0.000381>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000104>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000394>
1     [00007f67fa8b09a0] read(0, "l", 1) = 1 <0.107376>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000816>
1     [00007f67fa8b0a00] write(2, "l", 1) = 1 <0.000292>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000350>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000290>
1     [00007f67fa8b09a0] read(0, "o", 1) = 1 <0.156544>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000174>
1     [00007f67fa8b0a00] write(2, "o", 1) = 1 <0.000205>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000148>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000123>
1     [00007f67fa8b09a0] read(0, "\r", 1) = 1 <0.703854>
1     [00007f67fa8b0a00] write(2, "\n", 1) = 1 <0.000099>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000228>
1     [00007f67fa8b597c] ioctl(0, SNDCTL_TMR_STOP or TCSETSW, {B38400 opost isig icanon echo ...}) = 0 <0.000185>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000174>
1     [00007f67fa7f750d] rt_sigaction(SIGINT, {sa_handler=0x456bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000127>
1     [00007f67fa7f750d] rt_sigaction(SIGTERM, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000163>
1     [00007f67fa7f750d] rt_sigaction(SIGQUIT, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000172>
1     [00007f67fa7f750d] rt_sigaction(SIGALRM, {sa_handler=0x456da0, sa_mask=[HUP INT ILL TRAP ABRT BUS FPE USR1 SEGV USR2 PIPE ALRM TERM XCPU XFSZ VTALRM SYS], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000088>
1     [00007f67fa7f750d] rt_sigaction(SIGTSTP, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000022>
1     [00007f67fa7f750d] rt_sigaction(SIGTTOU, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000086>
1     [00007f67fa7f750d] rt_sigaction(SIGTTIN, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000063>
1     [00007f67fa7f750d] rt_sigaction(SIGWINCH, {sa_handler=0x456740, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=0x49ba70, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000074>
1     [00007f67fa8c0ae7] socket(AF_NETLINK, SOCK_RAW, NETLINK_AUDIT) = 3 <0.000071>
1     [00007f67fa8c09d0] sendmsg(3, {msg_name={sa_family=AF_NETLINK, nl_pid=0, nl_groups=00000000}, msg_namelen=12, msg_iov=[{iov_base={len=24, type=0x464 /* NLMSG_??? */, flags=NLM_F_REQUEST, seq=0, pid=0}, iov_len=16}, {iov_base="\x2e\x2f\x68\x65\x6c\x6c\x6f\x00", iov_len=8}], msg_iovlen=2, msg_controllen=0, msg_flags=0}, 0) = 24 <0.000074>
1     [00007f67fa8b1050] close(3)       = 0 <0.000054>
1     [00007f67fa7f750d] rt_sigaction(SIGINT, {sa_handler=0x456bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=0x456bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000046>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [INT CHLD], [], 8) = 0 <0.000046>
1     [00007f67fa8b1137] pipe([3, 4])   = 0 <0.000067>
1     [00007f67fa886972] clone( <unfinished ...>
79    [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [],  <unfinished ...>
1     [00007f67fa886972] <... clone resumed>child_stack=NULL, flags=CLONE_CHILD_CLEARTID|CLONE_CHILD_SETTID|SIGCHLD, child_tidptr=0x7f67fb1d7a10) = 79 <0.000772>
79    [00007f67fa7f7630] <... rt_sigprocmask resumed>NULL, 8) = 0 <0.000492>
1     [00007f67fa887aa7] setpgid(79, 79 <unfinished ...>
79    [00007f67fa7f750d] rt_sigaction(SIGTSTP, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400},  <unfinished ...>
1     [00007f67fa887aa7] <... setpgid resumed>) = 0 <0.000365>
79    [00007f67fa7f750d] <... rt_sigaction resumed>{sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000378>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [],  <unfinished ...>
79    [00007f67fa7f750d] rt_sigaction(SIGTTIN, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400},  <unfinished ...>
1     [00007f67fa7f7630] <... rt_sigprocmask resumed>NULL, 8) = 0 <0.000375>
79    [00007f67fa7f750d] <... rt_sigaction resumed>{sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000335>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [CHLD],  <unfinished ...>
79    [00007f67fa7f750d] rt_sigaction(SIGTTOU, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000037>
1     [00007f67fa7f7630] <... rt_sigprocmask resumed>[], 8) = 0 <0.000692>
79    [00007f67fa887aa7] setpgid(79, 79 <unfinished ...>
1     [00007f67fa8b1050] close(3 <unfinished ...>
79    [00007f67fa887aa7] <... setpgid resumed>) = 0 <0.000327>
1     [00007f67fa8b1050] <... close resumed>) = 0 <0.000315>
79    [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [CHLD TSTP TTIN TTOU],  <unfinished ...>
1     [00007f67fa8b1050] close(4 <unfinished ...>
79    [00007f67fa7f7630] <... rt_sigprocmask resumed>[], 8) = 0 <0.000356>
1     [00007f67fa8b1050] <... close resumed>) = 0 <0.000266>
79    [00007f67fa8b6307] ioctl(255, TIOCSPGRP, [79] <unfinished ...>
1     [00007f67fa8b6307] ioctl(255, TIOCGPGRP <unfinished ...>
79    [00007f67fa8b6307] <... ioctl resumed>) = 0 <0.000368>
1     [00007f67fa8b6307] <... ioctl resumed>, [79]) = 0 <0.000271>
79    [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [],  <unfinished ...>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [],  <unfinished ...>
79    [00007f67fa7f7630] <... rt_sigprocmask resumed>NULL, 8) = 0 <0.000578>
1     [00007f67fa7f7630] <... rt_sigprocmask resumed>NULL, 8) = 0 <0.000477>
79    [00007f67fa8b1050] close(4)       = 0 <0.000036>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [CHLD],  <unfinished ...>
79    [00007f67fa8b09a0] read(3,  <unfinished ...>
1     [00007f67fa7f7630] <... rt_sigprocmask resumed>[], 8) = 0 <0.000313>
79    [00007f67fa8b09a0] <... read resumed>"", 1) = 0 <0.000323>
1     [00007f67fa88646c] wait4(-1,  <unfinished ...>
79    [00007f67fa8b1050] close(3)       = 0 <0.000059>
79    [00007f67fa7f750d] rt_sigaction(SIGHUP, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, NULL, 8) = 0 <0.000036>
79    [00007f67fa7f750d] rt_sigaction(SIGILL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, NULL, 8) = 0 <0.000042>
79    [00007f67fa7f750d] rt_sigaction(SIGTRAP, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, NULL, 8) = 0 <0.000037>
79    [00007f67fa7f750d] rt_sigaction(SIGABRT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, NULL, 8) = 0 <0.000043>
79    [00007f67fa7f750d] rt_sigaction(SIGFPE, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, NULL, 8) = 0 <0.000058>
79    [00007f67fa7f750d] rt_sigaction(SIGBUS, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, NULL, 8) = 0 <0.000049>
79    [00007f67fa7f750d] rt_sigaction(SIGSEGV, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, NULL, 8) = 0 <0.000036>
79    [00007f67fa7f750d] rt_sigaction(SIGSYS, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, NULL, 8) = 0 <0.000047>
79    [00007f67fa7f750d] rt_sigaction(SIGPIPE, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, NULL, 8) = 0 <0.000039>
79    [00007f67fa7f750d] rt_sigaction(SIGALRM, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, NULL, 8) = 0 <0.000034>
79    [00007f67fa7f750d] rt_sigaction(SIGXCPU, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, NULL, 8) = 0 <0.000133>
79    [00007f67fa7f750d] rt_sigaction(SIGXFSZ, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, NULL, 8) = 0 <0.000075>
79    [00007f67fa7f750d] rt_sigaction(SIGVTALRM, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, NULL, 8) = 0 <0.000037>
79    [00007f67fa7f750d] rt_sigaction(SIGUSR1, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, NULL, 8) = 0 <0.000055>
79    [00007f67fa7f750d] rt_sigaction(SIGUSR2, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, NULL, 8) = 0 <0.000071>
79    [00007f67fa7f750d] rt_sigaction(SIGINT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=0x456bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000048>
79    [00007f67fa7f750d] rt_sigaction(SIGQUIT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000049>
79    [00007f67fa7f750d] rt_sigaction(SIGTERM, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000050>
79    [00007f67fa7f750d] rt_sigaction(SIGCHLD, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f67fa7f7400}, {sa_handler=0x4414a0, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000048>
79    [00007f67fa886c37] execve("./hello", ["./hello"], 0x1e119e0 /* 9 vars */) = 0 <0.125500>
79    [00007f0a08658aac] brk(NULL)      = 0x1f13000 <0.000266>
79    [00007f0a086597da] mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f0a08860000 <0.000451>
79    [00007f0a086596d7] access("/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory) <0.000335>
79    [00007f0a08659677] open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3 <0.000320>
79    [00007f0a08659604] fstat(3, {st_mode=S_IFREG|0644, st_size=11792, ...}) = 0 <0.000272>
79    [00007f0a086597da] mmap(NULL, 11792, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f0a0885d000 <0.000244>
79    [00007f0a08659787] close(3)       = 0 <0.000248>
79    [00007f0a08659677] open("/lib64/libc.so.6", O_RDONLY|O_CLOEXEC) = 3 <0.000260>
79    [00007f0a08659697] read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0`&\2\0\0\0\0\0@\0\0\0\0\0\0\0000\324 \0\0\0\0\0\0\0\0\0@\08\0\n\0@\0K\0J\0\6\0\0\0\5\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0000\2\0\0\0\0\0\0000\2\0\0\0\0\0\0\10\0\0\0\0\0\0\0\3\0\0\0\4\0\0\0\200\352\30\0\0\0\0\0\200\352\30\0\0\0\0\0\200\352\30\0\0\0\0\0\34\0\0\0\0\0\0\0\34\0\0\0\0\0\0\0\20\0\0\0\0\0\0\0\1\0\0\0\5\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\3200\34\0\0\0\0\0\3200\34\0\0\0\0\0\0\0 \0\0\0\0\0\1\0\0\0\6\0\0\0\3606\34\0\0\0\0\0\3606<\0\0\0\0\0\3606<\0\0\0\0\0\260Q\0\0\0\0\0\0\20\233\0\0\0\0\0\0\0\0 \0\0\0\0\0\2\0\0\0\6\0\0\0`k\34\0\0\0\0\0`k<\0\0\0\0\0`k<\0\0\0\0\0\360\1\0\0\0\0\0\0\360\1\0\0\0\0\0\0\10\0\0\0\0\0\0\0\4\0\0\0\4\0\0\0p\2\0\0\0\0\0\0p\2\0\0\0\0\0\0p\2\0\0\0\0\0\0D\0\0\0\0\0\0\0D\0\0\0\0\0\0\0\4\0\0\0\0\0\0\0\7\0\0\0\4\0\0\0\3606\34\0\0\0\0\0\3606<\0\0\0\0\0\3606<\0\0\0\0\0\20\0\0\0\0\0\0\0\240\0\0\0\0\0\0\0\20\0\0\0\0\0\0\0P\345td\4\0\0\0\234\352\30\0\0\0\0\0\234\352\30\0\0\0\0\0\234\352\30\0\0\0\0\0\344j\0\0\0\0\0\0\344j\0\0\0\0\0\0\4\0\0\0\0\0\0\0Q\345td\6\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\20\0\0\0\0\0\0\0R\345td\4\0\0\0\3606\34\0\0\0\0\0\3606<\0\0\0\0\0\3606<\0\0\0\0\0\209\0\0\0\0\0\0\209\0\0\0\0\0\0\1\0\0\0\0\0\0\0\4\0\0\0\24\0\0\0\3\0\0\0GNU\0\371\372\375\342\201\340\340\342\257E\221\32\320\372\21[d\302\316\250\4\0\0\0\20\0\0\0\1\0\0\0GNU\0\0\0\0\0\2\0\0\0\6\0\0\0 \0\0\0\0\0\0\0\363\3\0\0\7\0\0\0\0\1\0\0\16\0\0\0\0000\20D\240 \2\1\210\3\346\220\305E\214\0\300\0\10\0\5\200\0`\300\200\0\r\212\f\0\4\20\0\210D2\10.@\210T<, \0162H&\204\300\214\4\10\0\2\2\16\241\254\32\4f\300\0\3002\0\300\0P\1 \201\10\204\v  ($\0\4 Z\0\20X\200\312DB(\0\6\200\20\30B\0 @\200\0\tP\0Q\212@\20\0\0\0\0\10\0\0\21\20", 832) = 832 <0.000383>
79    [00007f0a08659604] fstat(3, {st_mode=S_IFREG|0755, st_size=2156272, ...}) = 0 <0.000314>
79    [00007f0a086597da] mmap(NULL, 3985920, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f0a08272000 <0.000109>
79    [00007f0a08659877] mprotect(0x7f0a08436000, 2093056, PROT_NONE) = 0 <0.000152>
79    [00007f0a086597da] mmap(0x7f0a08635000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1c3000) = 0x7f0a08635000 <0.000157>
79    [00007f0a086597da] mmap(0x7f0a0863b000, 16896, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f0a0863b000 <0.000124>
79    [00007f0a08659787] close(3)       = 0 <0.000134>
79    [00007f0a086597da] mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f0a0885c000 <0.000113>
79    [00007f0a086597da] mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f0a0885a000 <0.000120>
79    [00007f0a08640efd] arch_prctl(ARCH_SET_FS, 0x7f0a0885a740) = 0 <0.000090>
79    [00007f0a08659877] mprotect(0x7f0a08635000, 16384, PROT_READ) = 0 <0.000106>
79    [00007f0a08659877] mprotect(0x600000, 4096, PROT_READ) = 0 <0.000080>
79    [00007f0a08659877] mprotect(0x7f0a08861000, 4096, PROT_READ) = 0 <0.000066>
79    [00007f0a08659857] munmap(0x7f0a0885d000, 11792) = 0 <0.000070>
79    [00007f0a08361314] fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(136, 0), ...}) = 0 <0.000045>
79    [00007f0a0836ae2a] mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f0a0885f000 <0.000051>
79    [00007f0a08361a00] write(1, "hello, world\r\n", 14) = 14 <0.000094>
79    [00007f0a08337c09] exit_group(0)  = ?
79    [????????????????] +++ exited with 0 +++
1     [00007f67fa88646c] <... wait4 resumed>[{WIFEXITED(s) && WEXITSTATUS(s) == 0}], WSTOPPED|WCONTINUED, NULL) = 79 <0.194356>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [CHLD TSTP TTIN TTOU], [CHLD], 8) = 0 <0.000036>
1     [00007f67fa8b6307] ioctl(255, TIOCSPGRP, [1]) = 0 <0.000049>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [CHLD], NULL, 8) = 0 <0.000064>
1     [00007f67fa8b5a69] ioctl(255, TCGETS, {B38400 opost isig icanon echo ...}) = 0 <0.000042>
1     [00007f67fa8b6307] ioctl(255, TIOCGWINSZ, {ws_row=30, ws_col=110, ws_xpixel=0, ws_ypixel=0}) = 0 <0.000036>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000054>
1     [00007f67fa7f7630] --- SIGCHLD {si_signo=SIGCHLD, si_code=CLD_EXITED, si_pid=79, si_uid=0, si_status=0, si_utime=0, si_stime=0} ---
1     [00007f67fa88646c] wait4(-1, 0x7ffdc7754a50, WNOHANG|WSTOPPED|WCONTINUED, NULL) = -1 ECHILD (No child processes) <0.000036>
1     [00007f67fa7f7409] rt_sigreturn({mask=[]}) = 0 <0.000039>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000036>
1     [00007f67fa7f750d] rt_sigaction(SIGINT, {sa_handler=0x456bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=0x456bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000038>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000059>
1     [00007f67fa8b0a00] write(1, "\33]0;@3553859d9978:/code/demo/hello\7", 35) = 35 <0.000060>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [CHLD TSTP TTIN TTOU], [], 8) = 0 <0.000067>
1     [00007f67fa8b6307] ioctl(255, TIOCSPGRP, [1]) = 0 <0.000053>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000045>
1     [00007f67fa7f750d] rt_sigaction(SIGINT, {sa_handler=0x456bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=0x456bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000052>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000036>
1     [00007f67fa8b6307] ioctl(0, TIOCGWINSZ, {ws_row=30, ws_col=110, ws_xpixel=0, ws_ypixel=0}) = 0 <0.000036>
1     [00007f67fa8b6307] ioctl(0, TIOCSWINSZ, {ws_row=30, ws_col=110, ws_xpixel=0, ws_ypixel=0}) = 0 <0.000050>
1     [00007f67fa8b5a69] ioctl(0, TCGETS, {B38400 opost isig icanon echo ...}) = 0 <0.000038>
1     [00007f67fa8b597c] ioctl(0, SNDCTL_TMR_STOP or TCSETSW, {B38400 opost isig -icanon -echo ...}) = 0 <0.000040>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000045>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [INT QUIT ALRM TERM TSTP TTIN TTOU], [], 8) = 0 <0.000036>
1     [00007f67fa7f750d] rt_sigaction(SIGINT, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=0x456bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000048>
1     [00007f67fa7f750d] rt_sigaction(SIGTERM, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000036>
1     [00007f67fa7f750d] rt_sigaction(SIGTERM, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000051>
1     [00007f67fa7f750d] rt_sigaction(SIGQUIT, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000049>
1     [00007f67fa7f750d] rt_sigaction(SIGQUIT, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000058>
1     [00007f67fa7f750d] rt_sigaction(SIGALRM, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=0x456da0, sa_mask=[HUP INT ILL TRAP ABRT BUS FPE USR1 SEGV USR2 PIPE ALRM TERM XCPU XFSZ VTALRM SYS], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000047>
1     [00007f67fa7f750d] rt_sigaction(SIGTSTP, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000053>
1     [00007f67fa7f750d] rt_sigaction(SIGTSTP, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000048>
1     [00007f67fa7f750d] rt_sigaction(SIGTTOU, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000055>
1     [00007f67fa7f750d] rt_sigaction(SIGTTOU, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000047>
1     [00007f67fa7f750d] rt_sigaction(SIGTTIN, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000030>
1     [00007f67fa7f750d] rt_sigaction(SIGTTIN, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, {sa_handler=0x49c420, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000037>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000049>
1     [00007f67fa7f750d] rt_sigaction(SIGWINCH, {sa_handler=0x49ba70, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f67fa7f7400}, {sa_handler=0x456740, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f67fa7f7400}, 8) = 0 <0.000048>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000035>
1     [00007f67fa8b0a00] write(2, "[root@3553859d9978 hello]# ", 27) = 27 <0.000050>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000048>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000035>
1     [00007f67fa8b09a0] read(0,  <detached ...>

```

这里先看第一行：

- `1`表示进程号
- `[00007f67fa8b09a0]`表示调用的系统函数的地址
- `read(0, ".", 1)`表示调用的系统函数和函数参数
- `= 1`表示调用的系统函数的返回值
- `<1.955794>`表示调用的系统函数的执行时间

```
1     [00007f67fa8b09a0] read(0, ".", 1) = 1 <1.955794>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000311>
1     [00007f67fa8b0a00] write(2, ".", 1) = 1 <0.000459>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000352>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000450>
1     [00007f67fa8b09a0] read(0, "/", 1) = 1 <0.042319>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000302>
1     [00007f67fa8b0a00] write(2, "/", 1) = 1 <0.000283>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000283>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000254>
1     [00007f67fa8b09a0] read(0, "h", 1) = 1 <0.351256>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000344>
1     [00007f67fa8b0a00] write(2, "h", 1) = 1 <0.000543>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000289>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000172>
1     [00007f67fa8b09a0] read(0, "e", 1) = 1 <0.149488>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000084>
1     [00007f67fa8b0a00] write(2, "e", 1) = 1 <0.000296>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000174>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000253>
1     [00007f67fa8b09a0] read(0, "l", 1) = 1 <0.220069>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000223>
1     [00007f67fa8b0a00] write(2, "l", 1) = 1 <0.000381>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000104>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000394>
1     [00007f67fa8b09a0] read(0, "l", 1) = 1 <0.107376>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000816>
1     [00007f67fa8b0a00] write(2, "l", 1) = 1 <0.000292>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000350>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000290>
1     [00007f67fa8b09a0] read(0, "o", 1) = 1 <0.156544>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000174>
1     [00007f67fa8b0a00] write(2, "o", 1) = 1 <0.000205>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0 <0.000148>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, NULL, [], 8) = 0 <0.000123>
1     [00007f67fa8b09a0] read(0, "\r", 1) = 1 <0.703854>
1     [00007f67fa8b0a00] write(2, "\n", 1) = 1 <0.000099>
1     [00007f67fa7f7630] rt_sigprocmask(SIG_BLOCK, [INT], [], 8) = 0 <0.000228>
```

前面这一大段系统函数read和系统函数write，跟踪到的是终端1键盘输入`./hello`的过程。进程1是`/bin/bash`程序启动的，就是输如命令的终端。

```
[00007f67fa886972] <... clone resumed>child_stack=NULL, flags=CLONE_CHILD_CLEARTID|CLONE_CHILD_SETTID|SIGCHLD, child_tidptr=0x7f67fb1d7a10) = 79 <0.000772>
```

系统函数clone的作用是创建一个进程，这里进程1创建了进程79。

系统函数fock也可以创建一个进程。

```
<unfinished ...> 
```

这个标志表示产生了进程切换。

```
1     [00007f67fa88646c] wait4(-1,  <unfinished ...>
```

进程1调用系统函数wait4进入阻塞状态，切换到进程79运行。

```
79    [00007f67fa886c37] execve("./hello", ["./hello"], 0x1e119e0 /* 9 vars */) = 0 <0.125500>
```

进程79调用系统函数execve执行可执行文件hello。

execve系统函数会加载可执行文件的`.text`（程序指令）和`.data`（程序数据）到当前进程，并覆盖当前进程。

`["./hello"]`是参数值。`0x1e119e0 /* 9 vars */`是环境参数值。在linux中，环境参数是供所有应用程序使用的公共数据。

```
79    [00007f0a08659677] open("/lib64/libc.so.6", O_RDONLY|O_CLOEXEC) = 3 <0.000260>
```

加载hello程序所依赖的相关库文件`/lib64/libc.so.6`。其中`= 3`的3，是文件描述符，就是指当前进程在访问的文件，这个值一般大于等于0。

`/lib64/libc.so.6`是共享目标文件，也叫共享库、运行库、动态库。

用户程序，会调用运行库（CRT，C Runtime Library）。运行库封装了操作系统更底层的系统调用函数。

Linux、Windows、Mac，这些系统它们的底层接口都是不一样的，并且比较原始且底层，直接使用比较复杂，所以要封装这些比较底层的系统调用。

```
79    [00007f0a08361a00] write(1, "hello, world\r\n", 14) = 14 <0.000094>
```

调用系统函数write输出`hello, world\r\n`。参数值14表示字符串有14个字节。

c语言的stdio.h头文件声明的printf函数的底层实现最终调用的就是系统函数write，而write的具体实现就在libc.so.6库里。

系统调用是可以直接写在程序里的。参考代码`demo_c/demo_linux_c/write/write.c`。

系统函数write就是暴露出来的最底层的函数了，再往下就是驱动和硬件相关了。

```
79    [00007f0a08337c09] exit_group(0)  = ?
79    [????????????????] +++ exited with 0 +++
```

进程79调用系统函数exit_group(0)退出进程，参数值0是进程退出状态码，就是程序里return的0。

```
1     [00007f67fa88646c] <... wait4 resumed>[{WIFEXITED(s) && WEXITSTATUS(s) == 0}], WSTOPPED|WCONTINUED, NULL) = 79 <0.194356>
```

父进程1知道子进程79退出了，调用系统函数wait4回收退出的子进程79，并回收子进程的内存资源。

`WIFEXITED(s)`是个宏函数，`WEXITSTATUS(s)`拿到退出状态码。

至此整个hello程序执行结束。
