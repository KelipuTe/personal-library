## windows环境安装和配置golang

- 2021-11-17
- windows10
- go-1.17

### 下载

去官方网站（https://golang.org/dl），下一个windows环境的安装包。

本人这里下载到的安装包叫go1.17.3.windows-amd64.msi。

上面的网站打不开就用这个网站（https://golang.google.cn/dl）。

### 安装

运行安装包，选个合适的安装目录，然后一路下一步即可。

### 配置

- 在桌面右击我的电脑找到属性并点击。
- 在界面左侧找到高级系统设置并点击。
- 在界面下面找到环境变量并点击。
- 在界面的系统变量列表中找到path变量并点击编辑。
- 在path变量中添加go的bin目录。
- 本人的这个目录是`C:\go\bin`。

环境变量GOPATH在上面安装的时候已经配置好了，如果没有的话需要自行配置。

### go版本

#### go version

可以在win10的命令行窗口（cmd黑窗口），使用`go version`命令查看go版本信息。

如果上面的安装步骤成功，则会输出go版本号。

```
> C:\Users\Administrator>go version
go version go1.17.3 windows/amd64
```

### 环境配置

- `go env`命令输出当前go开发包的环境变量状态。
- `go env -w`命令用于设置go开发包的环境变量。

```
> C:\Users\Administrator>go env
set GO111MODULE=
set GOARCH=amd64
set GOBIN=
set GOCACHE=C:\Users\Administrator\AppData\Local\go-build
set GOENV=C:\Users\Administrator\AppData\Roaming\go\env
set GOEXE=.exe
set GOEXPERIMENT=
set GOFLAGS=
set GOHOSTARCH=amd64
set GOHOSTOS=windows
set GOINSECURE=
set GOMODCACHE=C:\Users\Administrator\go\pkg\mod
set GONOPROXY=
set GONOSUMDB=
set GOOS=windows
set GOPATH=C:\Users\Administrator\go
set GOPRIVATE=
set GOPROXY=https://proxy.golang.org,direct
set GOROOT=c:\go
set GOSUMDB=sum.golang.org
set GOTMPDIR=
set GOTOOLDIR=c:\go\pkg\tool\windows_amd64
set GOVCS=
set GOVERSION=go1.17.3
set GCCGO=gccgo
set AR=ar
set CC=gcc
set CXX=g++
set CGO_ENABLED=1
set GOMOD=NUL
set CGO_CFLAGS=-g -O2
set CGO_CPPFLAGS=
set CGO_CXXFLAGS=-g -O2
set CGO_FFLAGS=-g -O2
set CGO_LDFLAGS=-g -O2
set PKG_CONFIG=pkg-config
set GOGCCFLAGS=-m64 -mthreads -fno-caret-diagnostics -Qunused-arguments -fmessage-length=0 -fdebug-prefix-map=C:\Users\ADMINI~1\AppData\Local\Temp\go-build1826627288=/tmp/go-build -gno-record-gcc-switches
```

- GOARCH：处理器架构
- GOROOT：go开发包的安装目录
- GOPATH：当前工作目录，建议不要设置全局的GOPATH，而是随项目设置GOPATH
- GOPROXY：代理
- GOPRIVATE：私有库，不走代理

受网络影响，默认的代理可能连不上，可以换七牛云的代理。使用下面的命令设置代理：

```
> go env -w GO111MODULE=on
> go env -w GOPROXY=https://goproxy.cn,direct
```
