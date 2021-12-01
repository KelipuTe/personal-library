## gcc

- 2021-12-01

### 安装

```shell
yum -y install gcc
```

### 验证

安装好之后可以使用`gcc -v`命令查看版本信息。成功输出如下gcc版本信息就说明安装成功了。

```shell
> gcc -v
Using built-in specs.
COLLECT_GCC=gcc
COLLECT_LTO_WRAPPER=/usr/libexec/gcc/x86_64-redhat-linux/4.8.5/lto-wrapper
Target: x86_64-redhat-linux
Configured with: ../configure --prefix=/usr --mandir=/usr/share/man --infodir=/usr/share/info --with-bugurl=http://bugzilla.redhat.com/bugzilla --enable-bootstrap --enable-shared --enable-threads=posix --enable-checking=release --with-system-zlib --enable-__cxa_atexit --disable-libunwind-exceptions --enable-gnu-unique-object --enable-linker-build-id --with-linker-hash-style=gnu --enable-languages=c,c++,objc,obj-c++,java,fortran,ada,go,lto --enable-plugin --enable-initfini-array --disable-libgcj --with-isl=/builddir/build/BUILD/gcc-4.8.5-20150702/obj-x86_64-redhat-linux/isl-install --with-cloog=/builddir/build/BUILD/gcc-4.8.5-20150702/obj-x86_64-redhat-linux/cloog-install --enable-gnu-indirect-function --with-tune=generic --with-arch_32=x86-64 --build=x86_64-redhat-linux
Thread model: posix
gcc version 4.8.5 20150623 (Red Hat 4.8.5-44) (GCC)
```

### 参数

- `-E`：预处理
- `-o`：指定目标名称
- `-S`：编译成为汇编代码

### 编译

源码文件->gcc处理->elf可执行文件。

gcc处理包括：预处理（预处理器）、编译（编译器）、汇编（汇编器）、链接（连接器）。

- 预处理会增加一些代码。
- 编译得到汇编代码。
- 汇编把汇编代码转化为机器指令。
- 链接把各个模块连接起来组织成为可执行文件。

这里的hello.c文件是和gcc.md本篇同目录下的file.md那篇里的hello.c源码文件。

```shell
# 预处理
> gcc -E hello.c -o hello.i
> file hello.i
hello.i: C source, ASCII text
# 编译，得到汇编代码
> gcc -S hello.i -o hello.s
> file hello.s
hello.s: ASCII text
# 汇编，hello.o是一个relocatable（可重定位）文件
> gcc -c hello.s -o hello.o
> file hello.o
hello.o: ELF 64-bit LSB relocatable, x86-64, version 1 (SYSV), not stripped
# 全套处理，a.out是一个可执行文件
> gcc hello.c
> file a.out
a.out: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.32, BuildID[sha1]=a00dd2efbcbd8e23589373d89189b9785f0b7542, not stripped
```