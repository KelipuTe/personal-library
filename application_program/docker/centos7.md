## centos7

- 2021-12-07

### strace

使用`strace -p`命令跟踪进程时报错：

```
attach: ptrace(PTRACE_SEIZE): Operation not permitted
```

参考官方文档：The solution for enabling of ptrace and PTRACE_ATTACH in Docker Containers（https://bitworks.software/en/2017-07-24-docker-ptrace-attach.html）。

启动容器的时候使用`–-privileged`参数，让容器内的root用户拥有真正的root权限。

```
docker run -it -v {local path}:{docker path} --name={container name} --privileged centos:centos7
```

进入容器，然后使用命令：`echo 0 > /proc/sys/kernel/yama/ptrace_scope`，将`/proc/sys/kernel/yama/ptrace_scope`文件中的值修改成0。然后就可以使用`strace -p`命令跟踪进程了。