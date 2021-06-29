## linux环境使用php

- 文档更新日期：2021-06-29

### linux-alpine环境

首先找到php-config目录的位置，这个样例用的是swoole官方提供的docker镜像的alpine linux环境。在镜像phpswoole/swoole:4.4.23-php7.4中，php-config目录在`/usr/local/bin`目录下。

这里用mongodb扩展举例，使用命令下载mongodb扩展的tgz压缩包，也可以去pecl（[https://pecl.php.net/](https://pecl.php.net/)）上先下载好，再手动传到linux环境。

```shell
#解压
> tar zxfv mongodb-1.9.1.tgz
#进入解压后的目录
> cd mongodb-1.9.1
#生成configure配置文件
> phpize
#配置
> ./configure --with-php-config=/usr/local/bin/php-config
```

编译完成会看见下面的输出。

```shell
#编译
> make
...
----------------------------------------------------------------------
Libraries have been installed in:
   /code/mongodb-1.9.1/modules

If you ever happen to want to link against installed libraries
in a given directory, LIBDIR, you must either use libtool, and
specify the full pathname of the library, or use the `-LLIBDIR'
flag during linking and do at least one of the following:
   - add LIBDIR to the `LD_LIBRARY_PATH' environment variable
     during execution
   - add LIBDIR to the `LD_RUN_PATH' environment variable
     during linking
   - use the `-Wl,--rpath -Wl,LIBDIR' linker flag
   - have your system administrator add LIBDIR to `/etc/ld.so.conf'

See any operating system documentation about shared libraries for
more information, such as the ld(1) and ld.so(8) manual pages.
----------------------------------------------------------------------

Build complete.
```

安装完成会看见下面的输出，生成的so文件会放到下面这个目录里。

```shell
#安装
> make install
Installing shared extensions:     /usr/local/lib/php/extensions/no-debug-non-zts-20190902/
```

安装完成后在php-ini文件里加上mongodb的扩展mongodb.so。

使用`php --ini`命令找到配置文件。

```shell
> php --ini
Configuration File (php.ini) Path: /usr/local/etc/php
Loaded Configuration File:         /usr/local/etc/php/php.ini
Scan for additional .ini files in: /usr/local/etc/php/conf.d
Additional .ini files parsed:      /usr/local/etc/php/conf.d/docker-php-ext-sockets.ini,
/usr/local/etc/php/conf.d/docker-php-ext-sodium.ini,
/usr/local/etc/php/conf.d/docker-php-ext-swoole.ini
```

如果`Loaded Configuration File`这项是空的，就去`Configuration File (php.ini) Path`创建一个新的php.ini文件。通常这个目录下有php.ini-development这样的样例文件，复制一个就行了。