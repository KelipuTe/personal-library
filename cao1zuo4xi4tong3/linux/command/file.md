## file

- 2021-12-01

file命令用于辨识文件类型。

编写一个hello.c源码文件。

```c
#include <stdio.h>

void main() {
  printf("hello world\n");
}
```

```shell
# hello.c是一个C语言源码文件
> file hello.c
hello.c: C source, ASCII text, with CRLF line terminators
# 编译hello.c后，默认得到一个a.out文件
> gcc hello.c
# a.out是一个可执行文件
> file a.out
a.out: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.32, BuildID[sha1]=a00dd2efbcbd8e23589373d89189b9785f0b7542, not stripped
```