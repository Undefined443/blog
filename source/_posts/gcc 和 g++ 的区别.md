`gcc` 和 `g++` 是 GNU 编译器集合（GNU Compiler Collection，简称 GCC）中的两个不同命令，用于编译 C 和 C++ 代码。尽管它们都属于同一个编译器集合，但在处理 C 和 C++ 代码时有一些重要的区别。

## 主要区别

1. **默认语言处理**：
   - **`gcc`**：主要用于编译 C 语言代码。对于文件扩展名为 `.c` 的源文件，`gcc` 会将它们作为 C 代码处理。
   - **`g++`**：主要用于编译 C++ 语言代码。对于文件扩展名为 `.cpp`、`.cxx`、`.cc` 等的源文件，`g++` 会将它们作为 C++ 代码处理。

2. **链接阶段行为**：
   - **`gcc`**：在链接阶段不会自动链接 C++ 标准库。如果你需要编译和链接 C++ 代码，你需要手动指定链接 C++ 标准库。
   - **`g++`**：在链接阶段会自动链接 C++ 标准库。使用 `g++` 编译和链接 C++ 代码时，无需手动指定链接 C++ 标准库。

3. **文件名扩展名的处理**：
   - **`gcc`**：对于 .c 文件，会将其视为 C 代码进行编译。对于其他扩展名（如 .cpp、.cxx），需要使用 `-x` 选项指定语言类型。
   - **`g++`**：对于 .cpp、.cxx、.cc 文件，会将其视为 C++ 代码进行编译。对于 .c 文件，会将其视为 C++ 代码进行编译。

## 示例

### 使用 `gcc` 编译 C 代码

假设有一个简单的 C 程序 `hello.c`：

```c
#include <stdio.h>

int main() {
    printf("Hello, C World!\n");
    return 0;
}
```

编译和运行：

```sh
gcc hello.c -o hello
./hello
```

输出：

```
Hello, C World!
```

### 使用 `g++` 编译 C++ 代码

假设有一个简单的 C++ 程序 `hello.cpp`：

```cpp
#include <iostream>

int main() {
    std::cout << "Hello, C++ World!" << std::endl;
    return 0;
}
```

编译和运行：

```sh
g++ hello.cpp -o hello
./hello
```

输出：

```
Hello, C++ World!
```

### 使用 `gcc` 编译 C++ 代码（需要手动链接 C++ 标准库）

也可以使用 `gcc` 编译 C++ 代码，但需要手动指定链接 C++ 标准库：

```sh
gcc hello.cpp -o hello -lstdc++
./hello
```