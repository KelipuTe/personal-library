## 任务

- 文档更新时间：2020-10-19

### laravel schedule

这段脚本用于保证laravel的schedule定时任务，同时只有一个在跑。

```shell
#!/bin/bash

#查看是否有名字是schedule进程，有说明上一个还没跑完
ps auwx | grep 'schedule' | grep -v grep
#如果没有查到结果，说明没有在跑，就进入项目目录并执行schedule
if [ "$?" != "0" ]; then
cd /project/
php artisan schedule run > /dev/null 2>&1 &
fi
```
