## 程序

- 2021-12-04

### 程序是什么

程序是存储在磁盘上的静态代码文件或已经编译好的可执行文件。

### 代码和文件

- demo_c/demo_linux_c/hello/hello.c
- demo_c/demo_linux_c/hello/hello.o
- demo_c/demo_linux_c/hello/hello

### 编译

c源码文件通过gcc处理会得到elf可执行文件。gcc处理包括：

- 预处理（preprocessing），预处理器，预处理会增加一些代码。
- 编译（compilation），编译器，编译得到汇编代码。
- 汇编（assemble），汇编器，汇编把汇编代码转化为机器指令。
- 链接（link），连接器，链接把各个模块连接起来组织成为可执行文件。

在链接中，把函数的名字、变量的名字，都称为符号（symbol）。也就是说，源代码中的变量名和方法名，无论长度多少，最终都会转换为符号。

### hello.o

分别执行：

```
objdump -h hello.o
objdump -s hello.o
objdump -d hello.o
```

三个命令的结果第1行都会输出`file format elf64-x86-64v`，这个表示文件类型。

`objdump -h hello.o`

```
hello.o:     file format elf64-x86-64

Sections:
Idx Name          Size      VMA               LMA               File off  Algn
  0 .text         00000015  0000000000000000  0000000000000000  00000040  2**0
                  CONTENTS, ALLOC, LOAD, RELOC, READONLY, CODE
  1 .data         00000000  0000000000000000  0000000000000000  00000055  2**0
                  CONTENTS, ALLOC, LOAD, DATA
  2 .bss          00000000  0000000000000000  0000000000000000  00000055  2**0
                  ALLOC
  3 .rodata       0000000e  0000000000000000  0000000000000000  00000055  2**0
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
  4 .comment      0000002e  0000000000000000  0000000000000000  00000063  2**0
                  CONTENTS, READONLY
  5 .note.GNU-stack 00000000  0000000000000000  0000000000000000  00000091  2**0
                  CONTENTS, READONLY
  6 .eh_frame     00000038  0000000000000000  0000000000000000  00000098  2**3
                  CONTENTS, ALLOC, LOAD, RELOC, READONLY, DATA
```

- `.text`是代码段，它用于存储程序指令（程序的代码）。
- `.data`、`.bss`、`.rodata`是数据段，它们用于存储程序数据。
- `.bss`是未初始化数据段，`.rodata`是只读数据段。
- 程序指令+程序数据构成可执行文件。

`objdump -s hello.o`

```
hello.o:     file format elf64-x86-64

Contents of section .text:
 0000 554889e5 bf000000 00e80000 0000b800  UH..............
 0010 0000005d c3                          ...].
Contents of section .rodata:
 0000 68656c6c 6f2c2077 6f726c64 0d00      hello, world..
Contents of section .comment:
 0000 00474343 3a202847 4e552920 342e382e  .GCC: (GNU) 4.8.
 0010 35203230 31353036 32332028 52656420  5 20150623 (Red
 0020 48617420 342e382e 352d3434 2900      Hat 4.8.5-44).
Contents of section .eh_frame:
 0000 14000000 00000000 017a5200 01781001  .........zR..x..
 0010 1b0c0708 90010000 1c000000 1c000000  ................
 0020 00000000 15000000 00410e10 8602430d  .........A....C.
 0030 06500c07 08000000                    .P......
```

- `Contents of section .text`，每行从第2段开始，表示程序指令的内容（16进制机器指令）。后面的`UH............].`是程序指令的ascii文本。

`objdump -d hello.o`

```
hello.o:     file format elf64-x86-64


Disassembly of section .text:

0000000000000000 <main>:
   0:   55                      push   %rbp
   1:   48 89 e5                mov    %rsp,%rbp
   4:   bf 00 00 00 00          mov    $0x0,%edi
   9:   e8 00 00 00 00          callq  e <main+0xe>
   e:   b8 00 00 00 00          mov    $0x0,%eax
  13:   5d                      pop    %rbp
  14:   c3                      retq
```

- `Disassembly of section .text`，表示程序指令的内容，以及对应的汇编代码。这里和`objdump -s hello.o`的结果对应。
- `Contents of section .text`里的`554889e5`，就对应，`Disassembly of section .text`里前两行代码`0:   55`和`1:   48 89 e5`。
- `55`（0x55）反汇编后对应的汇编指令就是`push %rbp`。

### hello

用同样的方法，看一下可执行文件hello。

`objdump -h hello`

```
hello:     file format elf64-x86-64

Sections:
Idx Name          Size      VMA               LMA               File off  Algn
  0 .interp       0000001c  0000000000400238  0000000000400238  00000238  2**0
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
  1 .note.ABI-tag 00000020  0000000000400254  0000000000400254  00000254  2**2
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
  2 .note.gnu.build-id 00000024  0000000000400274  0000000000400274  00000274  2**2
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
  3 .gnu.hash     0000001c  0000000000400298  0000000000400298  00000298  2**3
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
  4 .dynsym       00000060  00000000004002b8  00000000004002b8  000002b8  2**3
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
  5 .dynstr       0000003d  0000000000400318  0000000000400318  00000318  2**0
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
  6 .gnu.version  00000008  0000000000400356  0000000000400356  00000356  2**1
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
  7 .gnu.version_r 00000020  0000000000400360  0000000000400360  00000360  2**3
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
  8 .rela.dyn     00000018  0000000000400380  0000000000400380  00000380  2**3
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
  9 .rela.plt     00000048  0000000000400398  0000000000400398  00000398  2**3
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
 10 .init         0000001a  00000000004003e0  00000000004003e0  000003e0  2**2
                  CONTENTS, ALLOC, LOAD, READONLY, CODE
 11 .plt          00000040  0000000000400400  0000000000400400  00000400  2**4
                  CONTENTS, ALLOC, LOAD, READONLY, CODE
 12 .text         00000182  0000000000400440  0000000000400440  00000440  2**4
                  CONTENTS, ALLOC, LOAD, READONLY, CODE
 13 .fini         00000009  00000000004005c4  00000000004005c4  000005c4  2**2
                  CONTENTS, ALLOC, LOAD, READONLY, CODE
 14 .rodata       0000001e  00000000004005d0  00000000004005d0  000005d0  2**3
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
 15 .eh_frame_hdr 00000034  00000000004005f0  00000000004005f0  000005f0  2**2
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
 16 .eh_frame     000000f4  0000000000400628  0000000000400628  00000628  2**3
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
 17 .init_array   00000008  0000000000600e10  0000000000600e10  00000e10  2**3
                  CONTENTS, ALLOC, LOAD, DATA
 18 .fini_array   00000008  0000000000600e18  0000000000600e18  00000e18  2**3
                  CONTENTS, ALLOC, LOAD, DATA
 19 .jcr          00000008  0000000000600e20  0000000000600e20  00000e20  2**3
                  CONTENTS, ALLOC, LOAD, DATA
 20 .dynamic      000001d0  0000000000600e28  0000000000600e28  00000e28  2**3
                  CONTENTS, ALLOC, LOAD, DATA
 21 .got          00000008  0000000000600ff8  0000000000600ff8  00000ff8  2**3
                  CONTENTS, ALLOC, LOAD, DATA
 22 .got.plt      00000030  0000000000601000  0000000000601000  00001000  2**3
                  CONTENTS, ALLOC, LOAD, DATA
 23 .data         00000004  0000000000601030  0000000000601030  00001030  2**0
                  CONTENTS, ALLOC, LOAD, DATA
 24 .bss          00000004  0000000000601034  0000000000601034  00001034  2**0
                  ALLOC
 25 .comment      0000002d  0000000000000000  0000000000000000  00001034  2**0
                  CONTENTS, READONLY
```

`objdump -s hello`

```
hello:     file format elf64-x86-64

Contents of section .interp:
 400238 2f6c6962 36342f6c 642d6c69 6e75782d  /lib64/ld-linux-
 400248 7838362d 36342e73 6f2e3200           x86-64.so.2.
Contents of section .note.ABI-tag:
 400254 04000000 10000000 01000000 474e5500  ............GNU.
 400264 00000000 02000000 06000000 20000000  ............ ...
Contents of section .note.gnu.build-id:
 400274 04000000 14000000 03000000 474e5500  ............GNU.
 400284 0e0456c8 e39e443d 78cd485c f270373f  ..V...D=x.H\.p7?
 400294 b03d6fed                             .=o.
Contents of section .gnu.hash:
 400298 01000000 01000000 01000000 00000000  ................
 4002a8 00000000 00000000 00000000           ............
Contents of section .dynsym:
 4002b8 00000000 00000000 00000000 00000000  ................
 4002c8 00000000 00000000 0b000000 12000000  ................
 4002d8 00000000 00000000 00000000 00000000  ................
 4002e8 10000000 12000000 00000000 00000000  ................
 4002f8 00000000 00000000 22000000 20000000  ........"... ...
 400308 00000000 00000000 00000000 00000000  ................
Contents of section .dynstr:
 400318 006c6962 632e736f 2e360070 75747300  .libc.so.6.puts.
 400328 5f5f6c69 62635f73 74617274 5f6d6169  __libc_start_mai
 400338 6e005f5f 676d6f6e 5f737461 72745f5f  n.__gmon_start__
 400348 00474c49 42435f32 2e322e35 00        .GLIBC_2.2.5.
Contents of section .gnu.version:
 400356 00000200 02000000                    ........
Contents of section .gnu.version_r:
 400360 01000100 01000000 10000000 00000000  ................
 400370 751a6909 00000200 31000000 00000000  u.i.....1.......
Contents of section .rela.dyn:
 400380 f80f6000 00000000 06000000 03000000  ..`.............
 400390 00000000 00000000                    ........
Contents of section .rela.plt:
 400398 18106000 00000000 07000000 01000000  ..`.............
 4003a8 00000000 00000000 20106000 00000000  ........ .`.....
 4003b8 07000000 02000000 00000000 00000000  ................
 4003c8 28106000 00000000 07000000 03000000  (.`.............
 4003d8 00000000 00000000                    ........
Contents of section .init:
 4003e0 4883ec08 488b050d 0c200048 85c07405  H...H.... .H..t.
 4003f0 e83b0000 004883c4 08c3               .;...H....
Contents of section .plt:
 400400 ff35020c 2000ff25 040c2000 0f1f4000  .5.. ..%.. ...@.
 400410 ff25020c 20006800 000000e9 e0ffffff  .%.. .h.........
 400420 ff25fa0b 20006801 000000e9 d0ffffff  .%.. .h.........
 400430 ff25f20b 20006802 000000e9 c0ffffff  .%.. .h.........
Contents of section .text:
 400440 31ed4989 d15e4889 e24883e4 f0505449  1.I..^H..H...PTI
 400450 c7c0c005 400048c7 c1500540 0048c7c7  ....@.H..P.@.H..
 400460 2d054000 e8b7ffff fff4660f 1f440000  -.@.......f..D..
 400470 b83f1060 0055482d 38106000 4883f80e  .?.`.UH-8.`.H...
 400480 4889e577 025dc3b8 00000000 4885c074  H..w.]......H..t
 400490 f45dbf38 106000ff e00f1f80 00000000  .].8.`..........
 4004a0 b8381060 0055482d 38106000 48c1f803  .8.`.UH-8.`.H...
 4004b0 4889e548 89c248c1 ea3f4801 d048d1f8  H..H..H..?H..H..
 4004c0 75025dc3 ba000000 004885d2 74f45d48  u.]......H..t.]H
 4004d0 89c6bf38 106000ff e20f1f80 00000000  ...8.`..........
 4004e0 803d4d0b 20000075 11554889 e5e87eff  .=M. ..u.UH...~.
 4004f0 ffff5dc6 053a0b20 0001f3c3 0f1f4000  ..]..:. ......@.
 400500 48833d18 09200000 741eb800 00000048  H.=.. ..t......H
 400510 85c07414 55bf200e 60004889 e5ffd05d  ..t.U. .`.H....]
 400520 e97bffff ff0f1f00 e973ffff ff554889  .{.......s...UH.
 400530 e5bfe005 4000e8d5 feffffb8 00000000  ....@...........
 400540 5dc3662e 0f1f8400 00000000 0f1f4000  ].f...........@.
 400550 41574189 ff415649 89f64155 4989d541  AWA..AVI..AUI..A
 400560 544c8d25 a8082000 55488d2d a8082000  TL.%.. .UH.-.. .
 400570 534c29e5 31db48c1 fd034883 ec08e85d  SL).1.H...H....]
 400580 feffff48 85ed741e 0f1f8400 00000000  ...H..t.........
 400590 4c89ea4c 89f64489 ff41ff14 dc4883c3  L..L..D..A...H..
 4005a0 014839eb 75ea4883 c4085b5d 415c415d  .H9.u.H...[]A\A]
 4005b0 415e415f c390662e 0f1f8400 00000000  A^A_..f.........
 4005c0 f3c3                                 ..
Contents of section .fini:
 4005c4 4883ec08 4883c408 c3                 H...H....
Contents of section .rodata:
 4005d0 01000200 00000000 00000000 00000000  ................
 4005e0 68656c6c 6f2c2077 6f726c64 0d00      hello, world..
Contents of section .eh_frame_hdr:
 4005f0 011b033b 34000000 05000000 10feffff  ...;4...........
 400600 80000000 50feffff 50000000 3dffffff  ....P...P...=...
 400610 a8000000 60ffffff c8000000 d0ffffff  ....`...........
 400620 10010000                             ....
Contents of section .eh_frame:
 400628 14000000 00000000 017a5200 01781001  .........zR..x..
 400638 1b0c0708 90010710 14000000 1c000000  ................
 400648 f8fdffff 2a000000 00000000 00000000  ....*...........
 400658 14000000 00000000 017a5200 01781001  .........zR..x..
 400668 1b0c0708 90010000 24000000 1c000000  ........$.......
 400678 88fdffff 40000000 000e1046 0e184a0f  ....@......F..J.
 400688 0b770880 003f1a3b 2a332422 00000000  .w...?.;*3$"....
 400698 1c000000 44000000 8dfeffff 15000000  ....D...........
 4006a8 00410e10 8602430d 06500c07 08000000  .A....C..P......
 4006b8 44000000 64000000 90feffff 65000000  D...d.......e...
 4006c8 00420e10 8f02450e 188e0345 0e208d04  .B....E....E. ..
 4006d8 450e288c 05480e30 8606480e 3883074d  E.(..H.0..H.8..M
 4006e8 0e406c0e 38410e30 410e2842 0e20420e  .@l.8A.0A.(B. B.
 4006f8 18420e10 420e0800 14000000 ac000000  .B..B...........
 400708 b8feffff 02000000 00000000 00000000  ................
 400718 00000000                             ....
Contents of section .init_array:
 600e10 00054000 00000000                    ..@.....
Contents of section .fini_array:
 600e18 e0044000 00000000                    ..@.....
Contents of section .jcr:
 600e20 00000000 00000000                    ........
Contents of section .dynamic:
 600e28 01000000 00000000 01000000 00000000  ................
 600e38 0c000000 00000000 e0034000 00000000  ..........@.....
 600e48 0d000000 00000000 c4054000 00000000  ..........@.....
 600e58 19000000 00000000 100e6000 00000000  ..........`.....
 600e68 1b000000 00000000 08000000 00000000  ................
 600e78 1a000000 00000000 180e6000 00000000  ..........`.....
 600e88 1c000000 00000000 08000000 00000000  ................
 600e98 f5feff6f 00000000 98024000 00000000  ...o......@.....
 600ea8 05000000 00000000 18034000 00000000  ..........@.....
 600eb8 06000000 00000000 b8024000 00000000  ..........@.....
 600ec8 0a000000 00000000 3d000000 00000000  ........=.......
 600ed8 0b000000 00000000 18000000 00000000  ................
 600ee8 15000000 00000000 00000000 00000000  ................
 600ef8 03000000 00000000 00106000 00000000  ..........`.....
 600f08 02000000 00000000 48000000 00000000  ........H.......
 600f18 14000000 00000000 07000000 00000000  ................
 600f28 17000000 00000000 98034000 00000000  ..........@.....
 600f38 07000000 00000000 80034000 00000000  ..........@.....
 600f48 08000000 00000000 18000000 00000000  ................
 600f58 09000000 00000000 18000000 00000000  ................
 600f68 feffff6f 00000000 60034000 00000000  ...o....`.@.....
 600f78 ffffff6f 00000000 01000000 00000000  ...o............
 600f88 f0ffff6f 00000000 56034000 00000000  ...o....V.@.....
 600f98 00000000 00000000 00000000 00000000  ................
 600fa8 00000000 00000000 00000000 00000000  ................
 600fb8 00000000 00000000 00000000 00000000  ................
 600fc8 00000000 00000000 00000000 00000000  ................
 600fd8 00000000 00000000 00000000 00000000  ................
 600fe8 00000000 00000000 00000000 00000000  ................
Contents of section .got:
 600ff8 00000000 00000000                    ........
Contents of section .got.plt:
 601000 280e6000 00000000 00000000 00000000  (.`.............
 601010 00000000 00000000 16044000 00000000  ..........@.....
 601020 26044000 00000000 36044000 00000000  &.@.....6.@.....
Contents of section .data:
 601030 00000000                             ....
Contents of section .comment:
 0000 4743433a 2028474e 55292034 2e382e35  GCC: (GNU) 4.8.5
 0010 20323031 35303632 33202852 65642048   20150623 (Red H
 0020 61742034 2e382e35 2d343429 00        at 4.8.5-44).
```

`objdump -d hello`

```
hello:     file format elf64-x86-64


Disassembly of section .init:

00000000004003e0 <_init>:
  4003e0:       48 83 ec 08             sub    $0x8,%rsp
  4003e4:       48 8b 05 0d 0c 20 00    mov    0x200c0d(%rip),%rax        # 600ff8 <__gmon_start__>
  4003eb:       48 85 c0                test   %rax,%rax
  4003ee:       74 05                   je     4003f5 <_init+0x15>
  4003f0:       e8 3b 00 00 00          callq  400430 <__gmon_start__@plt>
  4003f5:       48 83 c4 08             add    $0x8,%rsp
  4003f9:       c3                      retq

Disassembly of section .plt:

0000000000400400 <.plt>:
  400400:       ff 35 02 0c 20 00       pushq  0x200c02(%rip)        # 601008 <_GLOBAL_OFFSET_TABLE_+0x8>
  400406:       ff 25 04 0c 20 00       jmpq   *0x200c04(%rip)        # 601010 <_GLOBAL_OFFSET_TABLE_+0x10>
  40040c:       0f 1f 40 00             nopl   0x0(%rax)

0000000000400410 <puts@plt>:
  400410:       ff 25 02 0c 20 00       jmpq   *0x200c02(%rip)        # 601018 <puts@GLIBC_2.2.5>
  400416:       68 00 00 00 00          pushq  $0x0
  40041b:       e9 e0 ff ff ff          jmpq   400400 <.plt>

0000000000400420 <__libc_start_main@plt>:
  400420:       ff 25 fa 0b 20 00       jmpq   *0x200bfa(%rip)        # 601020 <__libc_start_main@GLIBC_2.2.5>
  400426:       68 01 00 00 00          pushq  $0x1
  40042b:       e9 d0 ff ff ff          jmpq   400400 <.plt>

0000000000400430 <__gmon_start__@plt>:
  400430:       ff 25 f2 0b 20 00       jmpq   *0x200bf2(%rip)        # 601028 <__gmon_start__>
  400436:       68 02 00 00 00          pushq  $0x2
  40043b:       e9 c0 ff ff ff          jmpq   400400 <.plt>

Disassembly of section .text:

0000000000400440 <_start>:
  400440:       31 ed                   xor    %ebp,%ebp
  400442:       49 89 d1                mov    %rdx,%r9
  400445:       5e                      pop    %rsi
  400446:       48 89 e2                mov    %rsp,%rdx
  400449:       48 83 e4 f0             and    $0xfffffffffffffff0,%rsp
  40044d:       50                      push   %rax
  40044e:       54                      push   %rsp
  40044f:       49 c7 c0 c0 05 40 00    mov    $0x4005c0,%r8
  400456:       48 c7 c1 50 05 40 00    mov    $0x400550,%rcx
  40045d:       48 c7 c7 2d 05 40 00    mov    $0x40052d,%rdi
  400464:       e8 b7 ff ff ff          callq  400420 <__libc_start_main@plt>
  400469:       f4                      hlt
  40046a:       66 0f 1f 44 00 00       nopw   0x0(%rax,%rax,1)

0000000000400470 <deregister_tm_clones>:
  400470:       b8 3f 10 60 00          mov    $0x60103f,%eax
  400475:       55                      push   %rbp
  400476:       48 2d 38 10 60 00       sub    $0x601038,%rax
  40047c:       48 83 f8 0e             cmp    $0xe,%rax
  400480:       48 89 e5                mov    %rsp,%rbp
  400483:       77 02                   ja     400487 <deregister_tm_clones+0x17>
  400485:       5d                      pop    %rbp
  400486:       c3                      retq
  400487:       b8 00 00 00 00          mov    $0x0,%eax
  40048c:       48 85 c0                test   %rax,%rax
  40048f:       74 f4                   je     400485 <deregister_tm_clones+0x15>
  400491:       5d                      pop    %rbp
  400492:       bf 38 10 60 00          mov    $0x601038,%edi
  400497:       ff e0                   jmpq   *%rax
  400499:       0f 1f 80 00 00 00 00    nopl   0x0(%rax)

00000000004004a0 <register_tm_clones>:
  4004a0:       b8 38 10 60 00          mov    $0x601038,%eax
  4004a5:       55                      push   %rbp
  4004a6:       48 2d 38 10 60 00       sub    $0x601038,%rax
  4004ac:       48 c1 f8 03             sar    $0x3,%rax
  4004b0:       48 89 e5                mov    %rsp,%rbp
  4004b3:       48 89 c2                mov    %rax,%rdx
  4004b6:       48 c1 ea 3f             shr    $0x3f,%rdx
  4004ba:       48 01 d0                add    %rdx,%rax
  4004bd:       48 d1 f8                sar    %rax
  4004c0:       75 02                   jne    4004c4 <register_tm_clones+0x24>
  4004c2:       5d                      pop    %rbp
  4004c3:       c3                      retq
  4004c4:       ba 00 00 00 00          mov    $0x0,%edx
  4004c9:       48 85 d2                test   %rdx,%rdx
  4004cc:       74 f4                   je     4004c2 <register_tm_clones+0x22>
  4004ce:       5d                      pop    %rbp
  4004cf:       48 89 c6                mov    %rax,%rsi
  4004d2:       bf 38 10 60 00          mov    $0x601038,%edi
  4004d7:       ff e2                   jmpq   *%rdx
  4004d9:       0f 1f 80 00 00 00 00    nopl   0x0(%rax)

00000000004004e0 <__do_global_dtors_aux>:
  4004e0:       80 3d 4d 0b 20 00 00    cmpb   $0x0,0x200b4d(%rip)        # 601034 <_edata>
  4004e7:       75 11                   jne    4004fa <__do_global_dtors_aux+0x1a>
  4004e9:       55                      push   %rbp
  4004ea:       48 89 e5                mov    %rsp,%rbp
  4004ed:       e8 7e ff ff ff          callq  400470 <deregister_tm_clones>
  4004f2:       5d                      pop    %rbp
  4004f3:       c6 05 3a 0b 20 00 01    movb   $0x1,0x200b3a(%rip)        # 601034 <_edata>
  4004fa:       f3 c3                   repz retq
  4004fc:       0f 1f 40 00             nopl   0x0(%rax)

0000000000400500 <frame_dummy>:
  400500:       48 83 3d 18 09 20 00    cmpq   $0x0,0x200918(%rip)        # 600e20 <__JCR_END__>
  400507:       00
  400508:       74 1e                   je     400528 <frame_dummy+0x28>
  40050a:       b8 00 00 00 00          mov    $0x0,%eax
  40050f:       48 85 c0                test   %rax,%rax
  400512:       74 14                   je     400528 <frame_dummy+0x28>
  400514:       55                      push   %rbp
  400515:       bf 20 0e 60 00          mov    $0x600e20,%edi
  40051a:       48 89 e5                mov    %rsp,%rbp
  40051d:       ff d0                   callq  *%rax
  40051f:       5d                      pop    %rbp
  400520:       e9 7b ff ff ff          jmpq   4004a0 <register_tm_clones>
  400525:       0f 1f 00                nopl   (%rax)
  400528:       e9 73 ff ff ff          jmpq   4004a0 <register_tm_clones>

000000000040052d <main>:
  40052d:       55                      push   %rbp
  40052e:       48 89 e5                mov    %rsp,%rbp
  400531:       bf e0 05 40 00          mov    $0x4005e0,%edi
  400536:       e8 d5 fe ff ff          callq  400410 <puts@plt>
  40053b:       b8 00 00 00 00          mov    $0x0,%eax
  400540:       5d                      pop    %rbp
  400541:       c3                      retq
  400542:       66 2e 0f 1f 84 00 00    nopw   %cs:0x0(%rax,%rax,1)
  400549:       00 00 00
  40054c:       0f 1f 40 00             nopl   0x0(%rax)

0000000000400550 <__libc_csu_init>:
  400550:       41 57                   push   %r15
  400552:       41 89 ff                mov    %edi,%r15d
  400555:       41 56                   push   %r14
  400557:       49 89 f6                mov    %rsi,%r14
  40055a:       41 55                   push   %r13
  40055c:       49 89 d5                mov    %rdx,%r13
  40055f:       41 54                   push   %r12
  400561:       4c 8d 25 a8 08 20 00    lea    0x2008a8(%rip),%r12        # 600e10 <__frame_dummy_init_array_entry>
  400568:       55                      push   %rbp
  400569:       48 8d 2d a8 08 20 00    lea    0x2008a8(%rip),%rbp        # 600e18 <__init_array_end>
  400570:       53                      push   %rbx
  400571:       4c 29 e5                sub    %r12,%rbp
  400574:       31 db                   xor    %ebx,%ebx
  400576:       48 c1 fd 03             sar    $0x3,%rbp
  40057a:       48 83 ec 08             sub    $0x8,%rsp
  40057e:       e8 5d fe ff ff          callq  4003e0 <_init>
  400583:       48 85 ed                test   %rbp,%rbp
  400586:       74 1e                   je     4005a6 <__libc_csu_init+0x56>
  400588:       0f 1f 84 00 00 00 00    nopl   0x0(%rax,%rax,1)
  40058f:       00
  400590:       4c 89 ea                mov    %r13,%rdx
  400593:       4c 89 f6                mov    %r14,%rsi
  400596:       44 89 ff                mov    %r15d,%edi
  400599:       41 ff 14 dc             callq  *(%r12,%rbx,8)
  40059d:       48 83 c3 01             add    $0x1,%rbx
  4005a1:       48 39 eb                cmp    %rbp,%rbx
  4005a4:       75 ea                   jne    400590 <__libc_csu_init+0x40>
  4005a6:       48 83 c4 08             add    $0x8,%rsp
  4005aa:       5b                      pop    %rbx
  4005ab:       5d                      pop    %rbp
  4005ac:       41 5c                   pop    %r12
  4005ae:       41 5d                   pop    %r13
  4005b0:       41 5e                   pop    %r14
  4005b2:       41 5f                   pop    %r15
  4005b4:       c3                      retq
  4005b5:       90                      nop
  4005b6:       66 2e 0f 1f 84 00 00    nopw   %cs:0x0(%rax,%rax,1)
  4005bd:       00 00 00

00000000004005c0 <__libc_csu_fini>:
  4005c0:       f3 c3                   repz retq

Disassembly of section .fini:

00000000004005c4 <_fini>:
  4005c4:       48 83 ec 08             sub    $0x8,%rsp
  4005c8:       48 83 c4 08             add    $0x8,%rsp
  4005cc:       c3                      retq
```

从上面三组结果可以看出，代码段`Disassembly of section .text`的第1个函数不是`main`函数，而是`_start`函数。`_start`函数会调用`__libc_start_main`函数进行一些必要的初始化操作，然后再调用`main`函数。