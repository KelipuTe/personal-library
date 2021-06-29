## lumen使用redis

- 文档更新时间：2021-07-07
- "laravel/lumen-framework":"v5.6.4"
- "predis/predis": "v1.1.1"
- "illuminate/redis": "v5.6.39"

### 安装redis扩展

```shell
> composer require predis/predis
> composer require illuminate/redis
```

安装时需要注意版本。直接执行上面的命令安装illuminate/redis时会安装最新版本的扩展，如果lumen是较低的版本，有可能会安装不成功。这个时候就需要指定illuminate/redis的版本。例如：lumen v5.6.4安装illuminate/redis扩展时，可以使用`> composer require illuminate/redis=v5.6.39`命令指定版本。

在bootstrap/app.php文件中注册Redis服务。

```php
//如果被注释了，需要打开注释
$app->withFacades();
//如果被注释了，需要打开注释
$app->withEloquent();
//注册redis服务
$app->register(Illuminate\Redis\RedisServiceProvider::class);
```

在`.env`文件中配置相关环境变量。这些参数会在config/database.php文件里配置Redis的地方被用到。

```
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=password
REDIS_DATABASE=0
```

### 使用redis

这个可以参考：[Laravel 大将之 Redis 模块](https://segmentfault.com/a/1190000009695841)。