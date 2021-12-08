## 常用命令

- 2021-11-17
- git2.34.0

### 配置用户名和邮箱

全局配置用户名和邮箱：
```
$ git config --global user.name "xxx"
$ git config --global user.email "xxx@xxx.com"
```

这条命令会在目录`C:\Users\用户名\`下创建一个`.gitconfig`文件用于保存配置。

### 修改提交地址

```
$ git remote set-url origin "xxx.git"
```