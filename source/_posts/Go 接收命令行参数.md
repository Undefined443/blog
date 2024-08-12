在 Go 语言中，可以使用标准库中的 `os` 包和 `flag` 包来接收和处理命令行参数。

### 使用 `os` 包

`os.Args` 是一个字符串切片，其中第一个元素是程序的名称，后续元素是传递给程序的命令行参数。

#### 示例代码

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    // os.Args[0] 是程序的名称
    // os.Args[1:] 是传递给程序的参数
    args := os.Args[1:]

    if len(args) == 0 {
        fmt.Println("No arguments provided.")
        return
    }

    fmt.Println("Received arguments:")
    for i, arg := range args {
        fmt.Printf("Argument %d: %s\n", i+1, arg)
    }
}
```

#### 运行方法

假设保存为 `main.go`，可以通过以下命令运行：

```sh
go run main.go arg1 arg2 arg3
```

输出：

```
Received arguments:
Argument 1: arg1
Argument 2: arg2
Argument 3: arg3
```

### 使用 `flag` 包

`flag` 包提供了更强大的命令行参数解析功能，支持带有默认值和帮助信息的参数。

#### 示例代码

```go
package main

import (
    "flag"
    "fmt"
)

func main() {
    // 定义命令行参数（参数名，默认值，参数说明）
    name := flag.String("name", "World", "a name to say hello to")
    age := flag.Int("age", 0, "your age")

    // 解析命令行参数
    flag.Parse()

    // 使用命令行参数
    fmt.Printf("Hello, %s!\n", *name)
    if *age > 0 {
        fmt.Printf("You are %d years old.\n", *age)
    }

    // 打印剩余的非标志参数
    fmt.Println("Remaining arguments:", flag.Args())
}
```

#### 运行方法

假设保存为 `main.go`，可以通过以下命令运行：

```sh
go run main.go -name Alice -age 30 additional_arg1 additional_arg2
```

输出：

```
Hello, Alice!
You are 30 years old.
Remaining arguments: [additional_arg1 additional_arg2]
```

### 说明

1. **定义参数**：使用 `flag.String`、`flag.Int` 等函数定义命令行参数。
2. **解析参数**：使用 `flag.Parse()` 解析命令行参数。
3. **使用参数**：通过解引用指针（如 `*name`）来获取参数的值。
4. **剩余参数**：可以使用 `flag.Args()` 获取解析后的剩余非标志参数。
