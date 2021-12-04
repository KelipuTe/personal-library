## ldd

- 2021-12-04

ldd可以列出一个程序所需要得动态链接库（so）。

```
> ldd hello
    linux-vdso.so.1 =>  (0x00007ffff2df3000)
    libc.so.6 => /lib64/libc.so.6 (0x00007f29997a2000)
    /lib64/ld-linux-x86-64.so.2 (0x00007f2999b70000)
```