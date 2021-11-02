## windows环境安装和配置Golang

- 文档更新时间：2021-07-16
- -windows10
- go-1.15

### 安装

下载安装包：[https://golang.google.cn/dl/](https://golang.google.cn/dl/)。这里下载的是：go1.15.3.windows-amd64.msi。

win10环境直接安装就行，这里安装目录选的是`E:\Go`。安装完成后在环境变量Path中添加`E:\Go\bin。`

打开cmd，输入命令`go version`，如果安装成功，则会输出go版本号，输出如下。

```shell
> go version

go version go1.15.3 windows/amd64
```

### 环境信息

`go env`命令输出当前go开发包的环境变量状态。

`go env -w`命令用于设置go开发包的环境变量。

```shell
> go env

set GO111MODULE=on
set GOARCH=amd64
set GOBIN=
set GOCACHE=C:\Users\Administrator\AppData\Local\go-build
set GOENV=C:\Users\Administrator\AppData\Roaming\go\env
set GOEXE=.exe
set GOFLAGS=
set GOHOSTARCH=amd64
set GOHOSTOS=windows
set GOINSECURE=
set GOMODCACHE=E:\Go\pkg\mod
set GONOPROXY=
set GONOSUMDB=
set GOOS=windows
set GOPATH=E:\Go
set GOPRIVATE=
set GOPROXY=https://goproxy.cn,direct
set GOROOT=E:\Go
set GOSUMDB=sum.golang.org
set GOTMPDIR=
set GOTOOLDIR=E:\Go\pkg\tool\windows_amd64
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
set GOGCCFLAGS=-m64 -mthreads -fmessage-length=0 -fdebug-prefix-map=C:\Users\ADMINI~1\AppData\Local\Temp\go-build582226306=/tmp/go-build -gno-record-gcc-switches
```

- GOARCH：处理器架构
- GOROOT：go开发包的安装目录
- GOPATH：当前工作目录，建议不要设置全局的GOPATH，而是随项目设置GOPATH
- GOPROXY：代理，`https://goproxy.cn/`
- GOPRIVATE：私有库，不走代理

使用命令设置代理：`go env -w GOPROXY=https://goproxy.cn,direct`
