在 Go 语言中，闭包（closure）是一个函数值，它引用了其外部作用域中的变量。简而言之，闭包能够“捕获”并“记住”其外部作用域中的变量，即使这个变量的生命周期已经结束。闭包的这种特性使得它在许多编程场景中非常有用，但也可能导致一些意外行为，尤其是在捕获变量时。

### 捕获问题的例子

一个常见的捕获问题是循环变量的捕获。下面是一个经典的例子：

```go
package main

import "fmt"

func main() {
    var funcs []func()

    for i := 0; i < 3; i++ {
        funcs = append(funcs, func() {
            fmt.Println(i)
        })
    }

    for _, f := range funcs {
        f()
    }
}
```

你可能期望这个程序输出 `0`, `1`, `2`，但实际上它会输出 `3`, `3`, `3`。原因在于闭包捕获的是变量 `i` 的引用，而不是它的值。循环结束后，`i` 的最终值是 `3`，因此所有的闭包都引用的是这个最终值。

### 解决方法

要解决这个问题，可以通过在每次迭代中创建一个新的变量来捕获当前的值：

```go
package main

import "fmt"

func main() {
    var funcs []func()

    for i := 0; i < 3; i++ {
        i := i // 重新声明一个新的 `i` 变量
        funcs = append(funcs, func() {
            fmt.Println(i)
        })
    }

    for _, f := range funcs {
        f()
    }
}
```

或者使用一个辅助函数：

```go
package main

import "fmt"

func main() {
    var funcs []func()

    for i := 0; i < 3; i++ {
        funcs = append(funcs, createFunc(i))
    }

    for _, f := range funcs {
        f()
    }
}

func createFunc(i int) func() {
    return func() {
        fmt.Println(i)
    }
}
```

在这两个示例中，每个闭包都捕获了不同的 `i` 值，这样就能得到期望的输出 `0`, `1`, `2`。

### 总结

闭包捕获变量时，容易出现捕获变量引用而不是值的问题。特别是在循环中，这种问题更容易发生。通过在每次迭代中创建一个新的变量或者使用辅助函数，可以有效地避免这种问题。理解闭包的捕获机制对于编写正确且高效的 Go 代码非常重要。