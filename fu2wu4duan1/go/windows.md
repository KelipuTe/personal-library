## win10 环境使用 go

- 文档更新时间：2021-07-16
- go-1.15

### GOROOT

指明go的安装目录的绝对路径。

### GOPATH

指明了go的工作区目录的绝对路径，需要在win10环境变量中显式的设置GOPATH。

多个工作区的绝对路径之间需要用分隔符`;`分隔。

### env

`go env`命令用于打印环境信息。

`go env -w`命令用于设置环境信息。

### module

go module是官方自带的go依赖管理库，在1.13版本正式推荐使用。

go1.13+的版本，判断module开不开启的依据是，根目录下有没有`go.mod`文件。

使用`go env -w GO111MODULE=on`命令可以设置module全局开启。

### GOPROXY

可以通过GOPROXY来控制代理，以及通过GOPRIVATE控制私有库（不走代理）。

使用`go env -w GOPROXY=https://goproxy.cn,direct`命令可以设置go模块代理为七牛云的代理。