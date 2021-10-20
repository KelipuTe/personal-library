## 操作相关

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

### mv

```shell
#重新创建日志文件并替换旧的日志文件
vim new.log
mv now.log old.log
mv new.log now.log
```
