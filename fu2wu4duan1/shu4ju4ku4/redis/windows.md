## windows环境安装和配置redis

- 2020-06-29
- windows10
- redis3.2.1

### 下载

去github下载一个windows环境的解压版的redis。注意github上有两个redis。

一个是[antirez/redis(https://github.com/antirez/redis)](https://github.com/antirez/redis)，这个不支持windows。

另一个是[MicrosoftArchive/redis(https://github.com/MicrosoftArchive/redis)](https://github.com/MicrosoftArchive/redis)，这个支持windows。

### 解压版安装

下载完成后解压到自定义目录。进入redis目录。

在redis目录找到redis.windows.conf配置文件。一般不需要修改什么配置，这里修改一下密码。

用记事本打开配置文件，全局搜索`requirepass`，找到以后去掉前面的`#`号，注意有空格的话也去掉，修改后面的密码，这样`root`就是密码了。

```
requirepass root
```

### 命令行启动

以管理员模式打开cmd，进入redis目录。

执行命令`redis-server redis.windows.conf`。使用配置文件的配置启动redis服务。

启动成功的话会看见这个。

```
                _._
           _.-``__ ''-._
      _.-``    `.  `_.  ''-._           Redis 3.2.100 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 11360
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

[11360] 21 Oct 15:01:14.951 # Server started, Redis version 3.2.100
[11360] 21 Oct 15:01:14.985 * DB loaded from disk: 0.033 seconds
[11360] 21 Oct 15:01:14.985 * The server is now ready to accept connections on port 6379
```

### 注册windows服务

以管理员模式打开cmd，进入redis目录，常用的操作有下面4个。

```shell
#安装redis服务
redis-server --service-install redis.windows.conf
#启动redis服务
redis-server --service-start
#停止redis服务
redis-server --service-stop
#卸载redis服务
redis-server --service-uninstall
```

执行命令`redis-server --service-install redis.windows.conf`安装的服务，可以从`右击我的电脑->管理-服务和应用程序->服务`中找到，名字一般就redis。

安装好后，需要手动执行第二行命令启动服务，也可以从刚才那个服务管理界面启动。

### 1067程序意外终止

安装成windows服务后，启动时报`1067程序意外终止`的解决方案。

1、记录日志

找到redis.windows.conf配置文件文件中的`logfile`参数。

如果设置为`logfile "logs/redis_log.txt"`，那么日志会被记录在`redis目录/logs/redis_log.txt`文件里。

2、重新安装一次redis服务并启动。日志文件里获得如下内容：

```
[8344] 22 Apr 11:15:18.166 # Granting read/write access to 'NT AUTHORITY\NetworkService' on: "C:\Study\Database\redis-x64-3.2.100" "C:\Study\Database\redis-x64-3.2.100\" 
[8344] 22 Apr 11:15:18.167 # Redis successfully installed as a service.
[7800] 22 Apr 11:15:23.512 # Creating Server TCP listening socket 127.0.0.1:6379: unable to bind socket
```

3、检查一下windows服务，看一下是不是网络服务。如果是网络服务，右击属性，改成本地服务。

### RedisDesktopManager

RedisDesktopManager是一个redis数据库桌面管理系统，类似navicate。

可以去github下载。[uglide/RedisDesktopManager(https://github.com/uglide/RedisDesktopManager)](https://github.com/uglide/RedisDesktopManager) 。

0.8.8版本一下不用付费。新版本的是需要付费的，有能力可以支持一下付费版。0.8.8版本地址[(https://github.com/uglide/RedisDesktopManager/releases/tag/0.8.8)](https://github.com/uglide/RedisDesktopManager/releases/tag/0.8.8)。
