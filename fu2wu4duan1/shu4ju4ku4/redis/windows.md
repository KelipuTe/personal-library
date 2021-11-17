## windows环境安装和配置redis

- 2021-11-17
- windows-10
- redis-5.0.14

### 下载

去github下载一个windows环境的解压版的redis。注意`redis/redis`（https://github.com/redis/redis）不支持windows。`tporadowski/redis`（https://github.com/tporadowski/redis）这个支持windows。

本人这里下载下来的文件是：Redis-x64-5.0.14.zip

### 配置

下载完成后解压到合适的位置。

配置文件redis.windows.conf在根目录。用记事本打开配置文件。

一般不需要修改什么配置，这里修改一下密码。

全局搜索`AUTH <PASSWORD>`，在下面几行的位置找到`requirepass`。

requirepass后面的就是设置的密码，去掉前面的`#`号启用，注意有空格的话也去掉。

### 命令行启动

以管理员模式打开cmd，进入redis目录。

执行命令`redis-server redis.windows.conf`启动redis服务并使用配置文件。

启动成功的话会看见这个。

```
[14904] 17 Nov 13:43:37.618 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
[14904] 17 Nov 13:43:37.618 # Redis version=5.0.14, bits=64, commit=a7c01ef4, modified=0, pid=14904, just started
[14904] 17 Nov 13:43:37.618 # Configuration loaded
                _._
           _.-``__ ''-._
      _.-``    `.  `_.  ''-._           Redis 5.0.14 (a7c01ef4/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 14904
  `-._    `-._  `-./  _.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |           http://redis.io
  `-._    `-._`-.__.-'_.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |
  `-._    `-._`-.__.-'_.-'    _.-'
      `-._    `-.__.-'    _.-'
          `-._        _.-'
              `-.__.-'

[14904] 17 Nov 13:43:37.624 # Server initialized
[14904] 17 Nov 13:43:37.625 * Ready to accept connections
```

### 注册服务

以管理员模式打开cmd，进入redis目录，常用的操作有下面4个。

```
#安装redis服务
redis-server --service-install redis.windows.conf
#启动redis服务
redis-server --service-start
#停止redis服务
redis-server --service-stop
#卸载redis服务
redis-server --service-uninstall
```

安装的服务，可以从`右击我的电脑->管理-服务和应用程序->服务`中找到，名字一般就是redis。

安装好后，需要手动执行第二行命令启动服务，也可以从刚才windows服务管理界面启动。
