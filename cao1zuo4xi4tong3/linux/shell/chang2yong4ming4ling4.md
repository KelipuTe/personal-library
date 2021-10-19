## 常用shell命令

- 文档更新时间：2021-10-19

### clear

clear命令可以清空命令行界面，通常和别的命令连用，达到清空界面并执行命令的目的。

比如`> clear && ps -auwx`命令，可以清空界面然后显示当前进程的状态。

`&&`表示逻辑与，和大多数编程语言中的用法一样，如果前面一个命令执行完成并返回真值就接着执行后面的命令。

### cp

```shell
#复制文件夹
cp -rf now/ now-time/
```

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

### mv

```shell
#重新创建日志文件并替换旧的日志文件
vim new.log
mv now.log old.log
mv new.log now.log
```

### nohup

nohup用于在系统后台不挂断地运行命令。

使用nohup命令运行应用程序，可以使其作为后台守护进程运行。

```shell
nohup php artisan serve --port=8000 > nohup_laravel.log 2>&1 &
```

- `nohup ... &`：在后台不挂断的执行命令。
- `php artisan serve --port=8000`：laravel服务的启动命令。
- `> nohup_laravel.log`：将命令的标准输出重定向到nohup_laravel.log文件
- `2>&1`：将标准错误输出重定向到标准输出。
- `> nohup_laravel.log 2>&1`：合起来用，就是把所有输出都定向到文件。

上面的命令执行后会返回进程id。

### tail

```shell
#读取fileName文件尾部n行
> tail -n fileName
#持续读取fileName文件尾部的数据
> tail -f fileName
```

