## 动态链接和静态链接

### 代码和文件

- demo_c/demo_linux_c/link/dynamically.c
- demo_c/demo_linux_c/link/dynamically
- demo_c/demo_linux_c/link/library.c
- demo_c/demo_linux_c/link/library.so
- demo_c/demo_linux_c/link/dynamically_gcc.c
- demo_c/demo_linux_c/link/dynamically_call.c

### 动态链接

通过`file dynamically`命令可以查看`dynamically`文件的文件类型。

```
dynamically: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.32, BuildID[sha1]=e5074c4bdb6869497fdecaed02a4061717424af3, not stripped
```

其中，`dynamically linked`，表示这个文件是动态链接的elf可执行程序。它必定有一个解释器，就是linux动态链接器。

有的时候从`file`命令的结果里可以直接看见。如果没有的话，也可以通过`readelf -l dynamically`命令，从elf文件的program headers中找到。

```
Program Headers:
  Type           Offset             VirtAddr           PhysAddr
                 FileSiz            MemSiz              Flags  Align
  PHDR           0x0000000000000040 0x0000000000400040 0x0000000000400040
                 0x00000000000001f8 0x00000000000001f8  R E    8
  INTERP         0x0000000000000238 0x0000000000400238 0x0000000000400238
                 0x000000000000001c 0x000000000000001c  R      1
      [Requesting program interpreter: /lib64/ld-linux-x86-64.so.2]
  LOAD           0x0000000000000000 0x0000000000400000 0x0000000000400000
                 0x0000000000000814 0x0000000000000814  R E    200000
  LOAD           0x0000000000000e10 0x0000000000600e10 0x0000000000600e10
                 0x000000000000022c 0x0000000000000238  RW     200000
  DYNAMIC        0x0000000000000e28 0x0000000000600e28 0x0000000000600e28
                 0x00000000000001d0 0x00000000000001d0  RW     8
  NOTE           0x0000000000000254 0x0000000000400254 0x0000000000400254
                 0x0000000000000044 0x0000000000000044  R      4
  GNU_EH_FRAME   0x00000000000006c4 0x00000000004006c4 0x00000000004006c4
                 0x000000000000003c 0x000000000000003c  R      4
  GNU_STACK      0x0000000000000000 0x0000000000000000 0x0000000000000000
                 0x0000000000000000 0x0000000000000000  RW     10
  GNU_RELRO      0x0000000000000e10 0x0000000000600e10 0x0000000000600e10
                 0x00000000000001f0 0x00000000000001f0  R      1
```

就是这个`interpreter: /lib64/ld-linux-x86-64.so.2`。它就是linux动态链接器。它的功能是，把ELF程序所依赖的动态库（共享库）加载并初始化。

当elf可执行程序的类型是动态链接的话，那么这个程序加载到内存运行时，操作系统会把控制权先交给动态链接器，动态链接器加载完该程序所依赖的动态库并初始化相应的必要操作后，才会把控制权交给程序的入口地址。也就是说，linux在使用`execve()`函数加载程序时，不管elf文件的具体类型是什么，只要检测出含有`.interp`段，那么，linux就会先运行动态链接器。运行完之后，才会将控制权交给elf程序的入口地址，如果这个程序没有`.interp`段，就会直接从程序的入口地址直接执行。

可以通过`ldd dynamically`命令查看`dynamically`文件依赖的动态库。

```
    linux-vdso.so.1 =>  (0x00007fffed1ed000)
    libc.so.6 => /lib64/libc.so.6 (0x00007f4f2ab0f000)
    /lib64/ld-linux-x86-64.so.2 (0x00007f4f2aedd000)
```

`/lib64/ld-linux-x86-64.so.2 (0x00007f4f2aedd000)`就是动态链接器。通过`file /lib64/ld-linux-x86-64.so.2`命令，可以查看它的文件类型。结果是：`/lib64/ld-linux-x86-64.so.2: symbolic link to ld-2.17.so`，是个软链接。`ld-2.17.so`是个特殊的共享库文件，它可以执行。

`/lib64/libc.so.6`是依赖的动态库。通过`file /lib64/libc.so.6`命令，可以查看它的文件类型。结果是：`/lib64/libc.so.6: symbolic link to libc-2.17.so`，也是个软链接。lib是动态库的前缀；c是动态库的名字；.so 是后缀；6是主版本号；17是次版本号。`libc-2.17.so`的全路径是`/usr/lib64/libc-2.17.so`。它是c的运行库，封装了很多操作系统的接口。例如：`write()`函数就在里面，可以在`libc-2.17.so`的符号表里找到。

动态库的查找路径：`/usr/lib`、`/usr/lib64`存储的是操作系统的共享库，`/usr/local/lib`、`/usr/local/lib64`存储的是第三方的共享库。

`linux-vdso.so.1 =>  (0x00007fffed1ed000)`，它是比较特殊的一个共享库，它不在文件系统中，而是在内核中。详见linux文档：`vdso(7) - overview of the virtual ELF dynamic shared object`。

### 静态链接

如果通过`file`命令可以查看静态链接的elf文件的话，结果中会有`statically linked`。静态链接的程序，会直接从程序的入口地址开始执行。

### 动态库

使用动态库的好处：程序可以以模块的形式独立开发，方便维护和升级。

#### 编写动态库

代码示例：`demo_c/demo_linux_c/link/library.c`。

编码完成后使用`gcc -fPIC -shared library.c -o library.so`命令，编译成动态库。

#### 编译时链接动态库

代码示例：`demo_c/demo_linux_c/link/dynamically_gcc.c`。

编码完成后使用`gcc dynamically_gcc.c ./library.so -o dynamically_gcc`命令，在编译时连接动态库。

#### 程序中显示调用动态库

代码示例：`demo_c/demo_linux_c/link/dynamically_call.c`。