## 网络间进程间通信

2021-12-16

---

### Socket

Socket（套接字），就是对网络中不同主机上的应用进程之间进行双向通信的端点的抽象。

套接字（在一些资料里还称为套接口，数据接口）其实就是一个文件，它是套接字文件描述符。文件描述符就是进程已经打开的文件。

### TCP

TCP（Transmission Control Protocol，传输控制协议）

相关的文档有：

- "socket(7)"，socket概述
- "ip(7)"，ipv4协议
- "tcp(7)"，tcp协议
- "socket(2)"，创建socket
- "bind(2)"，绑定地址到socket
- "listen(2)"，监听socket连接
- "connect(2)"、"accept(2)"，发起、接收socket连接
- "send(2)"、"recv(2)"，向socket发送数据、从socket读取数据
- "byteorder(3)"，主机字节序和网络字节序转换
- "getsockopt(2)"、"setsockopt(2)"，获取、设置socket选项

可以通过在linux系统中使用`man`命令查看相关文档或者访问在线文档（https://man7.org/linux/man-pages/index.html）。

tcp服务端和客户端示例：

一次性http服务端示例：

可重用的http服务端示例：