## windows10环境使用docker部署easyswoole

文档更新日期：2021-06-29

### 部署swoole

swoole官方文档中提供了官方的docker镜像`https://hub.docker.com/r/phpswoole/swoole`。

easyswoole官方文档中提到："如果没有特殊需求，请选择最新稳定版本开始下载(我这里是稳定版v4.4.23)"。所以这里也使用v4.4.23版本的swoole。

DockerHub页面右侧提供的命令`docker pull phpswoole/swoole`会默认拉取最新的镜像，这里我们需要指定版本的所以不能用上面的命令。

我们需要点击页面上的Tags标签，然后在下面找到php7.4版本swoole4.4.23版本的tag，tag右侧提供了拉取镜像的命令`docker pull phpswoole/swoole:4.4.23-php7.4`。打开PowerShell执命令行，拉取成功的话会是下面这样的输出。

```
PS C:\Users\Administrator> docker pull phpswoole/swoole:4.4.23-php7.4
4.4.23-php7.4: Pulling from phpswoole/swoole
6ec7b7d162b2: Pull complete
db606474d60c: Pull complete
afb30f0cd8e0: Pull complete
3bb2e8051594: Pull complete
e2b7fe41b468: Pull complete
9ab9906ce2f0: Pull complete
50fac8a5156f: Pull complete
1029765b1cd3: Pull complete
e9a21d7557c6: Pull complete
517180811701: Pull complete
b9c164c9c98f: Pull complete
Digest: sha256:5bd895677cbc73a06ea33239459bc4a07486d15025c1d1c805438a61c839dd32
Status: Downloaded newer image for phpswoole/swoole:4.4.23-php7.4
docker.io/phpswoole/swoole:4.4.23-php7.4
```

同时使用命令`docker images`也能看到可以使用的镜像列表。

```
PS C:\Users\Administrator> docker images
REPOSITORY         TAG             IMAGE ID       CREATED        SIZE
hello-world        latest          d1165f221234   3 months ago   13.3kB
phpswoole/swoole   4.4.23-php7.4   fb33e322751b   5 months ago   480MB
```

### 启动容器

```
docker run -it -p 127.0.0.1:9503:9503 -v E:\code:/code phpswoole/swoole:4.4.23-php7.4 /bin/bash
```

这个命令，将本机9503端口映射到容器9503端口，将本机E:\code目录映射到容器/code目录。

php版本

```
root@2bc0c5d254d5:/code# php -v
PHP 7.4.13 (cli) (built: Dec 18 2020 21:12:27) ( NTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
```

composer版本

```
root@2bc0c5d254d5:/code# composer -V
Composer version 1.10.19 2020-12-04 09:14:16
```

这个镜像安装了pecl，可以用pecl安装需要的扩展

```
root@2bc0c5d254d5:/code# pecl help version
PEAR Version: 1.10.12
PHP Version: 7.4.13
Zend Engine Version: 3.4.0
Running on: Linux 2bc0c5d254d5 5.10.25-linuxkit #1 SMP Tue Mar 23 09:27:39 UTC 2021 x86_64
```

