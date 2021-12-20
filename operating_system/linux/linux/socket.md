## 网络间进程间通信

2021-12-16

---

### Socket

Socket（套接字），就是对网络中不同主机上的应用进程之间进行双向通信的端点的抽象。

套接字（在一些资料里还称为套接口，数据接口）其实就是一个文件，它是套接字文件描述符。文件描述符就是进程已经打开的文件。

### TCP

TCP（Transmission Control Protocol，传输控制协议），可靠。

相关的文档有：

- "socket(7)"，socket概述
- "ip(7)"，ipv4协议
- "tcp(7)"，tcp协议
- "socket(2)"，创建socket
- "bind(2)"，绑定地址到socket
- "listen(2)"，监听socket连接
- "connect(2)"、"accept(2)"，发起、接收socket连接
- "send(2)"、"recv(2)"，向socket发送数据、从socket读取数据
- "byteorder(3)"、"inet(3)"，主机字节序和网络字节序转换
- "getsockopt(2)"、"setsockopt(2)"，获取、设置socket选项

可以通过在linux系统中使用`man`命令查看相关文档或者访问在线文档（https://man7.org/linux/man-pages/index.html）。

一次性的tcp服务端和客户端示例：`demo_c/demo_linux_c/socket/tcp10_server.c`、`demo_c/demo_linux_c/socket/tcp10_client.c`

一次性的http服务端示例：`demo_c/demo_linux_c/socket/tcp20_server.c`

可重用的http服务端示例：`demo_c/demo_linux_c/socket/tcp30_server.c`

### 进程的tcp socket网络表

相关的文档有："proc(5)"，进程信息伪文件系统。

启动tcp服务端tcp30_server.c，得到进程号。

查看`/proc/{pid}/fd/`目录。

```
total 0
lrwx------ 1 root root 64 Dec 18 05:53 0 -> /dev/pts/1
lrwx------ 1 root root 64 Dec 18 05:53 1 -> /dev/pts/1
lrwx------ 1 root root 64 Dec 18 05:53 2 -> /dev/pts/1
lrwx------ 1 root root 64 Dec 18 05:53 3 -> socket:[20443]
```

这个`3 -> socket:[20443]`，是socket函数创建的。

查看`/proc/{pid}/net/tcp`文件。

```
sl  local_address rem_address   st tx_queue rx_queue tr tm->when retrnsmt   uid  timeout inode

0: 00000000:251D 00000000:0000 0A 00000000:00000000 00:00000000 00000000     0        0 20443 1 0000000000000000 100 0 0 10 0
```

重要字段对应的解释：

- local_address，前面是本地ip，网络字节序，后面是本地端口，主机字节序
- rem_address，前面是远端ip，网络字节序，后面是远端端口，主机字节序
- st（connection state），套接字状态，16进制，具体解释看下面

st字段的状态（10进制）：

- TCP_ESTABLISHED:1   
- TCP_SYN_SENT:2
- TCP_SYN_RECV:3      
- TCP_FIN_WAIT1:4
- TCP_FIN_WAIT2:5     
- TCP_TIME_WAIT:6
- TCP_CLOSE:7         
- TCP_CLOSE_WAIT:8
- TCP_LAST_ACL:9     
- TCP_LISTEN:10
- TCP_CLOSING:11

使用telnet创建一个连接。

查看`/proc/{pid}/fd/`目录。

```
total 0
lrwx------ 1 root root 64 Dec 18 05:53 0 -> /dev/pts/1
lrwx------ 1 root root 64 Dec 18 05:53 1 -> /dev/pts/1
lrwx------ 1 root root 64 Dec 18 05:53 2 -> /dev/pts/1
lrwx------ 1 root root 64 Dec 18 05:53 3 -> socket:[20443]
lrwx------ 1 root root 64 Dec 18 05:53 4 -> socket:[20444]
```

这里多了一个`4 -> socket:[20444]`，是accept函数创建的。

查看`/proc/{pid}/net/tcp`文件。

```
sl  local_address rem_address   st tx_queue rx_queue tr tm->when retrnsmt   uid  timeout inode

0: 00000000:251D 00000000:0000 0A 00000000:00000000 00:00000000 00000000     0        0 20443 1 0000000000000000 100 0 0 10 0
1: 020011AC:C8D0 9D3A78CA:0050 06 00000000:00000000 03:00000E1B 00000000     0        0 0 3 0000000000000000
2: 0100007F:251D 0100007F:DA50 01 00000000:00000000 00:00000000 00000000     0        0 20444 1 0000000000000000 20 0 0 10 -1
3: 020011AC:A90E 03821CD2:0050 06 00000000:00000000 03:00000E1B 00000000     0        0 0 3 0000000000000000
4: 0100007F:DA50 0100007F:251D 01 00000000:00000000 00:00000000 00000000     0        0 20779 1 0000000000000000 20 0 0 10 -1
5: 020011AC:C8CE 9D3A78CA:0050 06 00000000:00000000 03:00000E1A 00000000     0        0 0 3 0000000000000000
6: 020011AC:8722 FB006F3B:0050 06 00000000:00000000 03:00000CB2 00000000     0        0 0 3 0000000000000000
```

这里多了telnet连接上来之后的一些数据，通过local_address和rem_address字段可以判断服务端和客户端

### UDP

UDP（User Datagram Protocol，用户数据包协议），不可靠，而且数据报会被重新排序。

相关的文档有：

- "socket(7)"，socket概述
- "ip(7)"，ipv4协议
- "udp(7)"，udp协议
- "socket(2)"，创建socket
- "bind(2)"，绑定地址到socket
- "sendto(2)"、"recvfrom(2)"，向socket发送数据、从socket读取数据
- "byteorder(3)"、"inet(3)"，主机字节序和网络字节序转换
- "getsockopt(2)"、"setsockopt(2)"，获取、设置socket选项

可以通过在linux系统中使用`man`命令查看相关文档或者访问在线文档（https://man7.org/linux/man-pages/index.html）。

注意udp服务端不能调用listen、connect、accept函数，因为udp是不建立连接的。

一次性的udp服务端和客户端示例：`demo_c/demo_linux_c/socket/udp10_server.c`、`demo_c/demo_linux_c/socket/udp10_client.c`。

可重用的udp服务端：`demo_c/demo_linux_c/socket/udp20_server.c`。

### 进程的udp socket网络表

基本和tcp那里差不多，区别是udp的文件是`/proc/{pid}/net/udp`。

### TCP和UDP的区别

tcp调用recv函数时，如果对端发送了多次，缓冲区有多少数据就读多少，不会丢失数据。

udp调用recvfrom函数时，如果对端发送了多次，后面的数据会被丢弃。当调用sendto和recvfrom次数一样时才有可能获取完整的数据。

### UNIX Socket

ipv4的tcp和udp需要走网卡，unix socket不需要。unix socket只能用于同一台机器上的进程间通信。

unix socket的类型也有tcp和udp两种，但是unix socket的udp是可靠的，而且数据报不会重新排序。

unix socket还分匿名的和命名的，这两个创建方式不一样，命名的还需要绑定。

相关的文档有：

- "unix(7)"，unix socket概述
- "socketpair(2)"，匿名unix socket
- 上文的tcp和udp相关文档

匿名unix socket示例：`unix10.c`

tcp unix socket示例：`unix20_tcp_server.c`、`unix20_tcp_client.c`

udp unix socket示例：