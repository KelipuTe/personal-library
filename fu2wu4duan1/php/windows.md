## windows10环境使用php

- 文档更新时间：2020-08-17

### 安装和配置

这里使用的是php-7.1.3版本。首先去官网下一个7.1.3版本的线程安全的压缩包php-7.1.3-Win32-VC14-x64.rar，解压压缩包后放到合适的位置。

在系统环境变量PATH中添加PHP所在的路径。例如：`D:\php;`和`D:\php\ext;`。前者是PHP的根目录，后者是PHP扩展的位置。

配置完PATH系统变量之后，我们需要配置php.ini文件。打开PHP根目录，复制一个php.ini-development，修改为php.ini。

打开php.ini，找到extension_dir这一行，修改为：`extension_dir = "D:\php\ext;"`。这个操作是指定PHP扩展的路径，必须指定扩展路径，否则php7启动不了。

配置完ext扩展目录之后，就可以在windows10的cmd命令行或者powershell中输入`> php -v`命令查看PHP版本信息了，也可以校验安装和配置是否成功。

### 配置PHP扩展

这里我用mysql扩展举例。在php.ini中找到相应的扩展。比如，mysql的扩展在php.ini文件中对应`extension=php_mysqli.dll`这一行，如果前面有分号说明这行代码是被注释的状态，去掉最前面的分号，启用扩展。

有些扩展（比如：redis扩展）不是压缩包自带的。需要去pecl（[https://pecl.php.net/](https://pecl.php.net/)）上下载扩展对应windows环境的`.dll`文件。然后把`.dll`文件放到扩展目录`D:\php\ext;`里去。配置方式和mysql是一样的，不过需要自己加一行`extension=`.dll文件名`，样例ini文件里没有这一行的。