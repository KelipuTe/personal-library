## window环境安装和配置git

- 2021-11-17
- win10
- git2.34.0

### 下载

去git官方网站（https://git-scm.com），下一个windows环境的安装包。

本人这里下载到的安装包叫Git-2.34.0-64-bit.exe。

### 安装

运行安装包，一路下一步即可。

### 记住密码

永久记住密码：

```
$ git config --global credential.helper store
```

临时记住密码：

```
$ git config –global credential.helper cache
$ git config –global credential.helper 'cache –timeout=3600'
```

这两条命令会在目录`C:\Users\用户名\`下创建一个`.gitconfig`文件用于保存配置。

第一次提交任然需要输入用户名和密码。提交成功后，同一个用户就不需要再输入了。

提交成功后会在目录`C:\Users\用户名\`下创建一个`.git-credentials`文件用于保存密码。

### 配置多个ssh

本人这里给github和阿里云云效两个代码仓库配置ssh。

ssh目录一般都在`~/.ssh`。win10上就是`C:\Users\用户名\.ssh`目录。

#### github配置ssh

生成ssh，用rsa算法。

```
$ ssh-keygen -t rsa -C "xxx@xxx.com" -f ~/.ssh/github_rsa
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/Administrator/.ssh/github_rsa
Your public key has been saved in /c/Users/Administrator/.ssh/github_rsa.pub
The key fingerprint is:
SHA256:xxx xxx@xxx.com
The key's randomart image is:cat 
```

获取公钥

```
$ cat ~/.ssh/github_rsa.pub
ssh-rsa 一大串 xxx@xxx.com
```

然后就把上面cat到的公钥内容贴到github代码仓库配置ssh的地方。

#### 阿里云云效配置ssh

生成ssh，云效要求使用ed25519算法。

```
$ ssh-keygen -t ed25519 -C "xxx@xxx.com" -f ~/.ssh/yun2xiao4_ed25519
Generating public/private ed25519 key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/Administrator/.ssh/yun2xiao4_ed25519
Your public key has been saved in /c/Users/Administrator/.ssh/yun2xiao4_ed25519.pub
The key fingerprint is:
SHA256:xxx xxx@xxx.com
The key's randomart image is:
```

获取公钥

```
$ cat ~/.ssh/yun2xiao4_ed25519.pub
ssh-ed25519 一大串 xxx@xxx.com
```

然后就把上面cat到的公钥内容贴到云效代码仓库配置ssh的地方。

#### 配置ssh

使用`ssh-add`命令添加私钥。

```
$ ssh-add ~/.ssh/github_rsa
Identity added: /c/Users/Administrator/.ssh/github_rsa (xxx@xxx.com)

$ ssh-add ~/.ssh/yun2xiao4_ed25519
Identity added: /c/Users/Administrator/.ssh/yun2xiao4_ed25519 (xxx@xxx.com)
```

如果遇到报错：`Could not open a connection to your authentication agent.`。

先执行`ssh-agent bash`命令，这命令没有输出，然后再执行`ssh-add`命令。

可以通过`ssh-add -l`命令查看私钥。可以通过`ssh-add -D`命令删除全部私钥。

私钥添加完成后在`.ssh`目录创建config配置文件。

在config文件添加以下内容，配置多个ssh就要写多个：

```
Host 识别模式，和主机名一样就行
HostName 主机名
Port 端口号，如果不是默认的22就需要指定
IdentityFile 秘钥文件路径
User 用户名
```
