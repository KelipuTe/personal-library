### bash脚本

- 文档更新时间：2020-06-18
- "laravel/framework":"v6.20.14"

### 启动laravel任务

在系统后台不挂断地启动laravel队列任务进程并输出进程输出到日志文件。

```shell
nohub /usr/local/php/bin/php artisan queue:work database --queue=queue_name >> /tmp/queue_output/queue_name.log &
```

### 自动重启laravel任务

这个脚本用laravel提供的artisan命令`queue:restart`平滑终止所有的队列任务，然后依次重新开启队列任务进程。

```bash
#!/bin/bash

cd /www/htdocs/code

/usr/local/php/bin/php artisan queue:restart
echo "queue:restart stop all porcess"

sleep 1

reload_queue_name=("job1" "job2")
for name in ${reload_queue_name[@]};
do
    nohup /usr/local/php/bin/php artisan queue:work database --queue=$name >> /tmp/queue_output/$name.log &
    echo "$name porcess start success"
done

exit;
```

### 自动重启laravel任务

这个脚本的思路是，根据队列任务名，依次找到进程，杀掉进程，然后重新开启队列任务进程。

```bash
#!/bin/bash

cd /www/htdocs/code

sleep 1

reload_queue_name=("job1" "job2")
for name in ${reload_queue_name[@]};
do
    ps -ef | grep $name | grep -v grep | awk '{print "kill "$2}' | sh
    echo "$name porcess kill success"
    nohup /usr/local/php/bin/php artisan queue:work database --queue=$name >> /tmp/queue_output/$name.log &
    echo "$name porcess start success"
done

exit;
```

