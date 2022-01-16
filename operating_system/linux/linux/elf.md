## ELF

2022-01-14

---

### 代码和文件

- demo_c/demo_linux_c/endian/endian.c
- demo_c/demo_linux_c/endian/objdump_d.log
- demo_c/demo_linux_c/endian/objdump_s.log
- demo_c/demo_linux_c/endian/program_headers.log
- demo_c/demo_linux_c/endian/readelf_s.log
- demo_c/demo_linux_c/endian/section_headers.log
- demo_c/demo_linux_c/endian/symbol.c

### ELF

详见linux文档：`elf(5) - format of Executable and Linking Format (ELF) files`。

Amongst these files are normal executable files（可执行文件）, relocatable object files（可重定位文件）, core files（core文件）, and shared objects（共享目标文件、共享库、动态库））.

### ELF Header

通过命令`readelf -h endian`，可以输出elf header（elf文件头）。

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
  Start of section headers:          6592 (bytes into file)
  Flags:                             0x0
  Size of this header:               64 (bytes)
  Size of program headers:           56 (bytes)
  Number of program headers:         9
  Size of section headers:           64 (bytes)
  Number of section headers:         30
  Section header string table index: 29
```

- `Data`：数据存储方式
- `Type`：elf文件的类型
- `Machine`：机器架构
- `Entry point address`：程序的入口地址。
- `Number of section headers`：elf段表大小

操作系统在加载完`endian`文件后，会把控制权转移给该程序，并找到入口地址`0x400440`开始运行。

### program headers

通过命令`readelf -l endian`，可以输出program headers（段表）。内容在文件`demo_c/demo_linux_c/endian/program_headers.log`中。

### section headers

通过命令`readelf -S endian`，可以输出section headers（段表）。内容在文件`demo_c/demo_linux_c/endian/section_headers.log`中。

- `.interp`：程序解释器（elf解释器）的路径。
- `.init`：进程初始化的代码。
- `.text`：程序指令-->指令码-->机器码-->二进制指令-->cpu指令（芯片层级的指令）。
- `.rodata`：只读数据（如：数字常量，字符串常量）。
- `.data`：已经给值的全局变量、静态变量、局部变量。
- `.bss`：未初始化的全局变量、静态变量、局部变量。
- `.symtab`：符号表。和`.strtab`一起用。
- `.strtab`：字符串符号表（变量名、函数名）。
- `.debug`：调试信息。
- 其他的段的解释详见linux文档。

### 段的大小

通过命令`size endian`，可以输出每段的大小，单位是字节。

```
   text    data     bss     dec     hex filename
   1483     556      12    2051     803 endian
```

### .text

在命令`objdump -s endian`的结果中，可以找到`.text`（代码段）。内容在文件`demo_c/demo_linux_c/endian/objdump_s.log`中。这里截取了最前面的一部分。

```
Contents of section .text:
 400440 31ed4989 d15e4889 e24883e4 f0505449  1.I..^H..H...PTI
 400450 c7c05006 400048c7 c1e00540 0048c7c7  ..P.@.H....@.H..
 400460 57054000 e8b7ffff fff4660f 1f440000  W.@.......f..D..
```

- 左边第1列是虚拟地址；中间的4列是指令码；最右边1列是ascii码
- `400440`是起始地址；`31ed4989`是起始指令；
- 指令是16进制的：`0x31 0xed 0x49 0x89`，大小为4个字节。

在命令`objdump -d endian`的结果中，可以找到对应的汇编代码。内容在文件文件`demo_c/demo_linux_c/endian/objdump_d.log`中。这里截取了上面的代码段对应的部分。通过虚拟地址的值和指令的值可以对应起来。

```
0000000000400440 <_start>:
  400440:	31 ed                	xor    %ebp,%ebp
  400442:	49 89 d1             	mov    %rdx,%r9
  400445:	5e                   	pop    %rsi
  400446:	48 89 e2             	mov    %rsp,%rdx
  400449:	48 83 e4 f0          	and    $0xfffffffffffffff0,%rsp
  40044d:	50                   	push   %rax
  40044e:	54                   	push   %rsp
  40044f:	49 c7 c0 50 06 40 00 	mov    $0x400650,%r8
  400456:	48 c7 c1 e0 05 40 00 	mov    $0x4005e0,%rcx
  40045d:	48 c7 c7 57 05 40 00 	mov    $0x400557,%rdi
  400464:	e8 b7 ff ff ff       	callq  400420 <__libc_start_main@plt>
  400469:	f4                   	hlt    
  40046a:	66 0f 1f 44 00 00    	nopw   0x0(%rax,%rax,1)
```

第1行的`_start`是函数名。

因为`400440`是起始地址，所以`_start`函数，就是这个程序的入口函数，它在调用`main`函数之前，会做一些前期的准备工作。

编程语言的`main`函数一般不是程序的入口函数，大部分入口函数都是类似`_start`函数这样的，而且不同的语言在初始化阶段会有各自的处理逻辑。

### .data

在命令`objdump -s endian`的结果中，可以找到`.data`（数据段）。内容在文件`demo_c/demo_linux_c/endian/objdump_s.log`中。

```
Contents of section .data:
 601030 00000000 2c010000 0a000000           ....,.......  
```

- 左边第1列是虚拟地址；后面的内容是数据。

比如，`endian.c`中定义了1个全局变量并给了值：`int a = 300;`。int占4个字节，300的16进制是`0x012c`（1 00101100），用4个字节的空间表示应该是`00000000 00000000 00000001 00101100`。

然后，因为数据使用主机字节序的格式存储的，所以在内存中应该是`00101100 00000001 00000000 00000000`，也就是`2c010000`。同理，`endian.c`中定义的静态变量：`static int i = 10;`。对应`0a000000`。

### 符号表

通过命令`readelf -s endian`，可以输出符号表。内容在文件`demo_c/demo_linux_c/endian/objdump_s.log`中。这里截取了`endian.c`中定义的部分变量对应的内容。

```
Symbol table '.symtab' contains 73 entries:
   Num:    Value          Size Type    Bind   Vis      Ndx Name
    42: 0000000000601038     4 OBJECT  LOCAL  DEFAULT   24 i.2184
    43: 0000000000601040     4 OBJECT  LOCAL  DEFAULT   25 j.2185
    54: 000000000040052d    42 FUNC    GLOBAL DEFAULT   13 func1
    56: 0000000000601044     4 OBJECT  GLOBAL DEFAULT   25 b
    67: 0000000000400440     0 FUNC    GLOBAL DEFAULT   13 _start
    68: 0000000000601034     4 OBJECT  GLOBAL DEFAULT   24 a
    70: 0000000000400557   124 FUNC    GLOBAL DEFAULT   13 main
```

这里用a举例，a在源码中是1个全局int变量，初始化为300。

- Name（符号名）：a
- Ndx（段id）：24，表示符号属于第24段。结合段表，第24段是`.data`段。
- Type（类型）：OBJECT，表示是个对象。如果是FUNC，就表示是个函数
- Bind（绑定范围）：GLOBAL，表示全局。
- Value（地址）：`0000000000601034`。

这里结合`.data`段的数据，地址`601034`等于`601030`+4，也就是上面的`2c010000`，值就是300。

另外，注意`_start`的值，它的值就是程序的入口地址。

命令`nm endian`，也可以输出，符号名和对应的内存地址。内容在文件`demo_c/demo_linux_c/endian/nm.log`中。

### 使用符号表的地址直接访问数据

在程序中可以直接使用符号表中的Value值去访问对应的内存数据。

同样的源文件，每次编译得到的符号表的地址是一样的。可以通过这种方式验证这个结论。

示例代码：`demo_c/demo_linux_c/endian/symbol.c`。

### 进程虚拟地址空间映射

操作系统会把内存地址和elf文件中的虚拟地址做映射。