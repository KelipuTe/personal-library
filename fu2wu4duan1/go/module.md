## win10 环境使用 go module

- 文档更新时间：2021-07-16
- go-1.15

### 初始化

在模块根目录下使用`go mod`命令进行初始化，初始化完成后会在模块根目录下生成`go.mod`文件。

```powershell
go mod init {module_name}
```

### 导入本地包

目录结构

```
- hello/
  - hello1/
    - helloworld1-1.go
  - hello2/
    - helloworld2-1.go
- main.go
- go.mod
```

go.mod

```go
module hello

go 1.15
```

main.go

```go
package main

import (
	"go0yang4li4/hello1"
	"go0yang4li4/hello2"
)

func main() {
	hello1.FHelloWorld()
	hello2.FHelloWorld()
}
```

helloworld1-1.go

```go
package hello1

import "fmt"

func FHelloWorld() {
	fmt.Println("hello world")
}
```

helloworld2-1.go

```go
package hello2

import "fmt"

func FHelloWorld() {
	fmt.Println("hello world")
}
```

