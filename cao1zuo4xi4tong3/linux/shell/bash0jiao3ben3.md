## bash脚本

- 文档更新时间：2020-10-19

### laravel schedule 定时任务

这段脚本用于保证laravel的schedule定时任务，同时只有一个在跑。

```shell
#!/bin/sh

#查看是否有名字是schedule进程，有说明上一个还没跑完
ps auwx | grep 'schedule' | grep -v grep
#如果没有查到结果，说明没有在跑，就进入项目目录并执行schedule
if [ "$?" != "0" ]; then
cd /project/
php artisan schedule run > /dev/null 2>&1 &
fi
```

### 每日日志压缩

这段脚本用于每天压缩前一天的日志文件。

```bash
#!/bin/bash

#获取前一天的yyyy-mm-dd格式的日期
dir_date=$(date -d yesterday +%Y-%m-%d)
#进入日志目录对应日期的目录
cd /tmp/logs/$dir_date/
#查找所有以.log为后缀名的文件，并压缩
find ./ -name '*.log' | xargs -i gzip {}

exit;
```

