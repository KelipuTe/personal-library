## 一些异常和解决方案

- 2020-06-29
- windows-10
- redis-3.2.1

### 1067程序意外终止

把redis安装成windows服务后，启动时报报错：1067程序意外终止。

解决方案如下：

1、记录日志

找到redis.windows.conf配置文件文件中的`logfile`参数。

如果设置为`logfile "logs/redis_log.txt"`，那么日志会被记录在`redis目录/logs/redis_log.txt`文件里。

2、重新安装一次redis服务并启动。日志文件里获得如下内容：

```
[8344] 22 Apr 11:15:18.166 # Granting read/write access to 'NT AUTHORITY\NetworkService' on: "D:\redis-x64-3.2.100" "D:\redis-x64-3.2.100\" 
[8344] 22 Apr 11:15:18.167 # Redis successfully installed as a service.
[7800] 22 Apr 11:15:23.512 # Creating Server TCP listening socket 127.0.0.1:6379: unable to bind socket
```

3、检查一下安装成的windows服务。看一下是不是网络服务。如果是网络服务，右击属性，改成本地服务。