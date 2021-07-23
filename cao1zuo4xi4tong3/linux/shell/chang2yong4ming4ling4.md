## 常用shell命令

- 文档更新时间：2021-09-08

### clear

clear命令可以清空命令行界面，通常和别的命令连用，达到清空界面并执行命令的目的。

比如`> clear && ps -auwx`命令，可以清空界面然后显示当前进程的状态。

`&&`表示逻辑与，和大多数编程语言中的用法一样，如果前面一个命令执行完成并返回真值就接着执行后面的命令。

### grep

grep命令用于查找文件里符合条件的字符串。

```shell
#在fileName文件中查找string，显示string所在行及后5行的数据
> grep -A 5 string fileName
#在fileName文件中查找string，显示string所在行及前5行的数据
> grep -B 5 string fileName
#在fileName文件中查找string，显示string所在行及前后5行的数据
> grep -C 5 string fileName
```

### lsof

```shell
#查看9503端口被哪个进程占用
> lsof -i:9503
```

### tail

```shell
#读取fileName文件尾部n行
> tail -n fileName
#持续读取fileName文件尾部的数据
> tail -f fileName
```

