## 网络间进程间通信

2021-12-16

---

### Socket

Socket（套接字），就是对网络中不同主机上的应用进程之间进行双向通信的端点的抽象。

套接字（在一些资料里还称为套接口，数据接口）其实就是一个文件，它是套接字文件描述符。文件描述符就是进程已经打开的文件。

详见：

- `socket(7) - Linux socket interface`
- `ip(7) - Linux IPv4 protocol implementation`

### TCP

TCP（Transmission Control Protocol，传输控制协议），可靠。

详见：`tcp(7) - TCP protocol`。

示例代码：

- 一次性的tcp服务端：`demo_c/demo_linux_c/socket/tcp/server_once.c`
- 一次性的tcp客户端：`demo_c/demo_linux_c/socket/tcp/client_once.c`
- 一次性的http服务端：`demo_c/demo_linux_c/socket/http/server_once.c`
- 可重用的http服务端：`demo_c/demo_linux_c/socket/http/server_multiple.c`

### 进程的tcp socket网络表

启动tcp服务端`server_multiple.c`，得到进程号。

查看`/proc/{pid}/fd/`目录。

```
total 0
lrwx------ 1 root root 64 Dec 18 05:53 0 -> /dev/pts/1
lrwx------ 1 root root 64 Dec 18 05:53 1 -> /dev/pts/1
lrwx------ 1 root root 64 Dec 18 05:53 2 -> /dev/pts/1
lrwx------ 1 root root 64 Dec 18 05:53 3 -> socket:[20443]
```

这个`3 -> socket:[20443]`，是`socket()`函数创建的。

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

使用`telnet`命令创建一个连接，`telnet 127.0.0.1 9501`。

查看`/proc/{pid}/fd/`目录。

```
total 0
lrwx------ 1 root root 64 Dec 18 05:53 0 -> /dev/pts/1
lrwx------ 1 root root 64 Dec 18 05:53 1 -> /dev/pts/1
lrwx------ 1 root root 64 Dec 18 05:53 2 -> /dev/pts/1
lrwx------ 1 root root 64 Dec 18 05:53 3 -> socket:[20443]
lrwx------ 1 root root 64 Dec 18 05:53 4 -> socket:[20444]
```

这里多了一个`4 -> socket:[20444]`，是`accept()`函数创建的。

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

这里多了`telnet`命令连接上来之后的一些数据，通过local_address和rem_address字段可以判断服务端和客户端

### UDP

UDP（User Datagram Protocol，用户数据包协议），不可靠，而且数据报会被重新排序。

详见：`udp(7) - User Datagram Protocol for IPv4`。

注意udp服务端不能调用`listen()`、`connect()`、`accept()`函数，因为udp是不建立连接的。

示例代码：

- 一次性的udp服务端：`demo_c/demo_linux_c/socket/udp/server_once.c`
- 一次性的udp客户端：`demo_c/demo_linux_c/socket/udp/client_once.c`
- 可重用的udp服务端：`demo_c/demo_linux_c/socket/udp/server_multiple.c`

### 进程的udp socket网络表

基本和tcp那里差不多，区别是udp的文件是`/proc/{pid}/net/udp`。

### TCP和UDP的区别

tcp调用`recv()`函数时，如果对端发送了多次，缓冲区有多少数据就读多少，不会丢失数据。

udp调用`recvfrom()`函数时，如果对端发送了多次，后面的数据会被丢弃。当发送端调用`sendto()`函数次数和接收端调用`recvfrom()`函数次数一样时才有可能获取完整的数据。

### UNIX Socket

unix socket只能用于同一台机器上的进程间通信。ipv4的tcp和udp需要走网卡，unix socket不需要。

unix socket的类型也有tcp和udp两种，但是unix socket的udp是可靠的，而且数据报不会重新排序。

unix socket还分匿名的和命名的，这两个创建方式不一样，命名的还需要绑定。

详见：`unix(7) - sockets for local interprocess communication`。

示例代码：

- 匿名unix socket：`demo_c/demo_linux_c/unix_socket/unnamed/unnamed.c`
- 一次性的tcp服务端：`demo_c/demo_linux_c/unix_socket/tcp/server_once.c`
- 一次性的tcp客户端：`demo_c/demo_linux_c/unix_socket/tcp/client_once.c`
- 一次性的udp服务端：`demo_c/demo_linux_c/unix_socket/udp/server_once.c`
- 一次性的udp客户端：`demo_c/demo_linux_c/unix_socket/udp/client_once.c`