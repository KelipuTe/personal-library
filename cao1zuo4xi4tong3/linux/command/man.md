## man

- 2021-12-01

man命令可以查看帮助手册。

### 安装

```
yum install man
yum install man-pages
```

### docker容器中无法使用

因为docker的镜像它将一些东西精简了，所以不能够使用一些常用的命令，比如man等等。

需要在/etc/yum.conf配置文件里面修改一下，注释掉：`tsflags=nodocs`，这个是禁用了一些软件包。

注释完成之后重新安装man即可使用。