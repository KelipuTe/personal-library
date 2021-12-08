## strace

- 2021-12-04

strace常用来跟踪进程执行时的系统调用和所接收的信号。

### 安装

```shell
yum -y install strace
```

### 参数

- `-f`：跟踪子进程
- `-i`：打印系统调用的地址
- `-o`：输出到文件
- `-p`：指定跟踪的进程号
- `-s`：每一行的长度，默认32，一般设置65535
- `-t`：每一行打印时间
- `-T`：显示系统调用花费的时间

### docker相关问题

详见：`/application_program/docker/centos7.md`