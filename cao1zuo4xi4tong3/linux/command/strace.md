## strace

- 2021-12-04

strace常用来跟踪进程执行时的系统调用和所接收的信号。

### 安装

```
yum -y install strace
```

### 参数

- `-f`：跟踪子进程
- `-i`：打印系统调用的地址
- `-o`：输出到文件
- `-p`：指定跟踪的进程号
- `-s`：每一行的长度，默认32，65535
- `-t`：每一行打印时间
- `-T`：显示系统调用花费的时间

### docker容器中无法使用

使用`strace -p`跟踪进程时报错：

```
attach: ptrace(PTRACE_SEIZE): Operation not permitted
```

参考官方文档：The solution for enabling of ptrace and PTRACE_ATTACH in Docker Containers（https://bitworks.software/en/2017-07-24-docker-ptrace-attach.html）。

启动容器的时候使用`–-privileged`参数，让容器内的root用户拥有真正的root权限。

```
docker run -it -v D:/code:/code --name=test01 --privileged centos:centos7
```

进入容器，然后使用命令：`echo 0 > /proc/sys/kernel/yama/ptrace_scope`，将`/proc/sys/kernel/yama/ptrace_scope`文件中的值修改成0。然后就可以使用`strace -p`跟踪进程了。
