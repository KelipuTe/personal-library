## 查询相关

- 文档更新时间：2021-10-19

### grep

grep命令用于查找文件里符合条件的字符串。通常和管道`|`一起用。

```shell
#在fileName文件中查找string，显示string所在行及后5行的数据
grep -A 5 string fileName
#在fileName文件中查找string，显示string所在行及前5行的数据
grep -B 5 string fileName
#在fileName文件中查找string，显示string所在行及前后5行的数据
grep -C 5 string fileName
```

### tail

```shell
#读取fileName文件尾部n行
tail -n fileName
#持续读取fileName文件尾部的数据
tail -f fileName
```

### lsof

```shell
#查看9503端口被哪个进程占用
lsof -i:9503
```

### hostnamectl

查看操作系统和操作系统的架构。

### lscpu

查看cpu相关信息。
