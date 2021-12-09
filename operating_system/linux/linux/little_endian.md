## 小端字节序（little endian）

- 2021-12-06

### 代码和文件

- demo_c/demo_linux_c/little_endian/little_endian.c
- demo_c/demo_linux_c/little_endian/little_endian.o
- demo_c/demo_linux_c/little_endian/little_endian
- demo_c/demo_linux_c/hello/hello

### little_endian.o

`objdump -s little_endian.o`

```
little_endian.o:     file format elf64-x86-64

Contents of section .text:
 0000 554889e5 4883ec20 897dec8b 45ec8945  UH..H.. .}..E..E
 0010 fc8b45fc 89c6bf00 000000b8 00000000  ..E.............
 0020 e8000000 008b45fc c9c35548 89e58b05  ......E...UH....
 0030 00000000 89c7e800 000000b8 00000000  ................
 0040 5dc3                                 ].
Contents of section .data:
 0000 2c010000 0a000000                    ,.......
Contents of section .rodata:
 0000 25640d0a 00                          %d...
Contents of section .comment:
 0000 00474343 3a202847 4e552920 342e382e  .GCC: (GNU) 4.8.
 0010 35203230 31353036 32332028 52656420  5 20150623 (Red
 0020 48617420 342e382e 352d3434 2900      Hat 4.8.5-44).
Contents of section .eh_frame:
 0000 14000000 00000000 017a5200 01781001  .........zR..x..
 0010 1b0c0708 90010000 1c000000 1c000000  ................
 0020 00000000 2a000000 00410e10 8602430d  ....*....A....C.
 0030 06650c07 08000000 1c000000 3c000000  .e..........<...
 0040 00000000 18000000 00410e10 8602430d  .........A....C.
 0050 06530c07 08000000                    .S......
```

`objdump -d little_endian.o`

```
little_endian.o:     file format elf64-x86-64


Disassembly of section .text:

0000000000000000 <func1>:
   0:   55                      push   %rbp
   1:   48 89 e5                mov    %rsp,%rbp
   4:   48 83 ec 20             sub    $0x20,%rsp
   8:   89 7d ec                mov    %edi,-0x14(%rbp)
   b:   8b 45 ec                mov    -0x14(%rbp),%eax
   e:   89 45 fc                mov    %eax,-0x4(%rbp)
  11:   8b 45 fc                mov    -0x4(%rbp),%eax
  14:   89 c6                   mov    %eax,%esi
  16:   bf 00 00 00 00          mov    $0x0,%edi
  1b:   b8 00 00 00 00          mov    $0x0,%eax
  20:   e8 00 00 00 00          callq  25 <func1+0x25>
  25:   8b 45 fc                mov    -0x4(%rbp),%eax
  28:   c9                      leaveq
  29:   c3                      retq

000000000000002a <main>:
  2a:   55                      push   %rbp
  2b:   48 89 e5                mov    %rsp,%rbp
  2e:   8b 05 00 00 00 00       mov    0x0(%rip),%eax        # 34 <main+0xa>
  34:   89 c7                   mov    %eax,%edi
  36:   e8 00 00 00 00          callq  3b <main+0x11>
  3b:   b8 00 00 00 00          mov    $0x0,%eax
  40:   5d                      pop    %rbp
  41:   c3                      retq
```

程序中的300和10是存储在`.data`数据段中的。分别对应`2c010000`和`0a000000`。但是300和10的16进制分别是`0000012c`和`0000000a`，这里是反的。这是因为在内存中是使用小端字节序存储的。小端字节序也成为主机字节序。

c语言的int变量由4个字节组成，每个字节由8个bit位组成。把10进制的300转换成2进制就是`00000000 00000000 00000001 00101100`，左边定义为高位，右边定义为低位。而内存是从低到高的，所以存储格式是把数据的低位放在内存低位上，就变成了`00101100 00000001 00000000 00000000`，转换成16进制就是`2c010000`。

## hello

`readelf -h hello`

```
ELF Header:
  Magic:   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00
  Class:                             ELF64
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              EXEC (Executable file)
  Machine:                           Advanced Micro Devices X86-64
  Version:                           0x1
  Entry point address:               0x400440
  Start of program headers:          64 (bytes into file)
  Start of section headers:          6440 (bytes into file)
  Flags:                             0x0
  Size of this header:               64 (bytes)
  Size of program headers:           56 (bytes)
  Number of program headers:         9
  Size of section headers:           64 (bytes)
  Number of section headers:         30
  Section header string table index: 29
```

在readelf命令的结果的`Data`字段里可以看见`little endian`。