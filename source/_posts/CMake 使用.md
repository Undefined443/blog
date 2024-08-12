## 常用工作流

```sh
mkdir build && cd build # 创建并进入 build 目录
cmake ..                # 生成 Makefile
make                    # 开始构建
```

## 常用 CMake 命令

```sh
cmake .                            # 使用 CMakeLists.txt 生成 Makefile
cmake -DCMAKE_BUILD_TYPE=Release . # 可以提供配置参数
cmake -S . -B build                # 也可以指定源文件目录（S）和构建目录（B）

cmake --build build                # 调用构建工具完成构建过程

cmake --install build              # 将产物安装到 CMakeLists.txt 指定的位置
```

配置参数也可以通过交互式命令行工具 `ccmake` 修改：

```sh
ccmake .
```

- 按回车修改配置，按 `c` 生成，按 `q` 退出。
- 修改完记得生成 Makefile 。

## CMake 语句

在 CMake 中，构建产物（可执行文件 executable 和库文件 library）被称为目标（target）。

```cmake
# 设置 CMake 版本，应该作为第一条语句
cmake_minimum_required(VERSION 3.25)

# 设置项目名称、语言、版本号，应该紧跟着 cmake_minimum_required 命令
project(Tutorial VERSION 1.0) # 指定项目名和版本号，将隐式创建 Tutorial_VERSION_MAJOR 变量和 Tutorial_VERSION_MINOR 变量

# 设置变量，应该在 add_executable 之前
set(CMAKE_CXX_STANDARD 17) # 设置 C++ 标准
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 构建可执行文件
add_executable(Tutorial tutorial.cpp) # 使用源文件 tutorial.cpp 构建可执行文件 Tutorial

# 设置目标的头文件搜索路径
target_include_directories(Tutorial PUBLIC include) # 设置 Tutorial 的头文件搜索路径为 include。PUBLIC 表示所有依赖于 Tutorial 的目标都会继承这个目录
```

`add_executable()` 定义了可执行文件对源代码的依赖关系

设置静态编译的方法：

```cmake
set(CMAKE_EXE_LINKER_FLAGS "-static") # 添加 -static 编译标记
```

> - 静态链接可以防止你的程序在其他机器上找不到动态库的情况，但是会使得程序体积变大。
> - Qt 程序不用设置静态编译，因为 Qt 程序除了要链接 C++ 标准库之外，还要链接 Qt 库，而 Qt 库无法通过 C++ 编译器直接静态链接。在 Windows 平台发布 Qt 程序之前应该先使用 `windeployqt` 工具将依赖的 Qt 动态库复制到程序目录下。

CMake 教程：[CMake Tutorial | cmake.org](https://cmake.org/cmake/help/latest/guide/tutorial/index.html)

参考文档：

- [cmake_minimum_required()](https://cmake.org/cmake/help/latest/command/cmake_minimum_required.html#command:cmake_minimum_required)：指定最低 CMake 版本。

- [project()](https://cmake.org/cmake/help/latest/command/project.html#command:project)：设置项目名和版本号

- [add_executable()](https://cmake.org/cmake/help/latest/command/add_executable.html#command:add_executable)：添加一个要构建的可执行文件

- [set()](https://cmake.org/cmake/help/latest/command/set.html#command:set)：设置变量

- [target_include_directories()](https://cmake.org/cmake/help/latest/command/target_include_directories.html#command:target_include_directories)：设置构建目标的头文件目录

### 使用模板文件

模板文件 Tutorial.h.in：

```cpp
#define Tutorial_VERSION_MAJOR @Tutorial_VERSION_MAJOR@
#define Tutorial_VERSION_MINOR @Tutorial_VERSION_MINOR@
```

> 变量 `@Tutorial_VERSION_MAJOR@` 和 `@Tutorial_VERSION_MINOR@` 是在 `project()` 命令中指定版本号时自动生成的

在 CMakeLists.txt 中指定模板文件和输出文件：

```cmake
configure_file(TutorialConfig.h.in TutorialConfig.h)
```

cmake 会将模板文件 `TutorialConfig.h.in` 中 `@VAR@` 形式的变量替换为变量的值。

[configure_file()](https://cmake.org/cmake/help/latest/command/configure_file.html#command:configure_file)

参考：[Step 1: A Basic Starting Point (Exercise 3 - Adding a Version Number and Configured Header File) | cmake.org](https://cmake.org/cmake/help/latest/guide/tutorial/A%20Basic%20Starting%20Point.html#exercise-3-adding-a-version-number-and-configured-header-file)

### 构建库

库（Library）通常作为一个独立的编译单元，因此通常为其创建一个子项目进行编译。在这里我们创建了子目录 MathFunctions 并在其中创建了子项目的 CMakeLists.txt 。

MathFunctions/CMakeLists.txt

```cmake
# 构建库文件
add_library(MathFunctions MathFunctions.cpp mysqrt.cpp)
```

项目根目录的 CMakeLists.txt：

```cmake
# 将子目录添加到构建过程
add_subdirectory(MathFunctions)

# 指定目标的头文件搜索路径
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           "${PROJECT_SOURCE_DIR}/MathFunctions")

# 将库文件 MathFunctions 链接到 Tutorial
target_link_libraries(Tutorial PUBLIC MathFunctions)
```

main.cpp

```cpp
#include "MathFunctions.h"

int main() {
    const double outputValue = mathfunctions::sqrt(inputValue);
}
```

- [add_subdirectory()](https://cmake.org/cmake/help/latest/command/add_subdirectory.html#command:add_subdirectory)
- [add_library()](https://cmake.org/cmake/help/latest/command/add_library.html#command:add_library)
- [target_link_libraries()](https://cmake.org/cmake/help/latest/command/target_include_directories.html#command:target_include_directories)

参考：[Step 2: Adding a Library (Exercise 1 - Creating a Library) | cmake.org](https://cmake.org/cmake/help/latest/guide/tutorial/Adding%20a%20Library.html#exercise-1-creating-a-library)

### 设置编译选项

CMakeLists.txt

```cmake
add_library(MathFunctions MathFunctions.cpp mysqrt.cpp)

# 定义布尔选项
option(USE_MYMATH "Use tutorial provided math implementation" ON)

if (USE_MYMATH)
    # 为目标定义预处理器宏 "USE_MYMATH"，PRIVATE 表示这些宏将只应用于目标自身
    target_compile_definitions(MathFunctions PRIVATE "USE_MYMATH")

    # 构建静态库
    add_library(SqrtLibrary STATIC mysqrt.cpp)

    # 将 SqrtLibrary 库链接到 MathFunctions 库
    target_link_libraries(MathFunctions PRIVATE SqrtLibrary)
endif()
```

MathFunctions.cpp

```cpp
#include "MathFunctions.h"

#ifdef USE_MYMATH
    #include "mysqrt.h"
#else
    #include <cmath>
#endif

namespace mathfunctions {
double sqrt(double x) {
    #ifdef USE_MYMATH
    return detail::mysqrt(x);
    #else
    return std::sqrt(x);
    #endif
}
}  // namespace mathfunctions
```

参考：[Step 2: Adding a Library (Exercise 2 - Adding an Option) | cmake.org](https://cmake.org/cmake/help/latest/guide/tutorial/Adding%20a%20Library.html#exercise-2-adding-an-option)

### 定义安装规则

通常，仅构建可执行文件是不够的，它还应该是可安装的。在 CMake 中,可以使用 `install()` 命令指定安装规则。

> 安装其实就是一种复制。

MathFunctions/CMakeLists.txt

```cmake
# 设置一个变量 installable_libs 并赋值
set(installable_libs MathFunctions tutorial_compiler_flags)

if(TARGET SqrtLibrary)                        # 如果定义了 SqrtLibrary
    list(APPEND installable_libs SqrtLibrary) # 向变量 installable_libs 添加 SqrtLibrary
endif()

# 安装目标 installable_libs 到 lib 目录
install(TARGETS ${installable_libs} DESTINATION lib)

# 安装文件 MathFunctions.h 到 include 目录
install(FILES MathFunctions.h DESTINATION include)
```

CMakeLists.txt

```cmake
# 安装目标 Tutorial 到 bin 目录
install(TARGETS Tutorial DESTINATION bin)

# 安装文件 TutorialConfig.h 到 include 目录
install(FILES "${PROJECT_BINARY_DIR}/TutorialConfig.h"
        DESTINATION include)
```

参考：[Step 5: Installing and Testing (Exercise 1 - Install Rules) | cmake.org](https://cmake.org/cmake/help/latest/guide/tutorial/Installing%20and%20Testing.html#exercise-1-install-rules)

### 自动化测试

```cmake
# 启用测试
enable_testing()

# 添加测试项目 Runs，测试命令为 Tutorial 25
add_test(NAME Runs COMMAND Tutorial 25)

# 添加测试项目 Usage，测试命令为 Tutorial
add_test(NAME Usage COMMAND Tutorial)

# 使用正则表达式验证测试结果
set_tests_properties(Usage
                     PROPERTIES PASS_REGULAR_EXPRESSION "Usage:.*number")

# 添加测试项目 StandardUse，测试命令为 Tutorial 4
add_test(NAME StandardUse COMMAND Tutorial 4)

# 使用正则表达式验证测试结果
set_tests_properties(StandardUse
                     PROPERTIES PASS_REGULAR_EXPRESSION "4 is 2")
```

可以创建一个函数来简化测试语句：

```cmake
function(do_test target arg result)
    add_test(NAME Comp${arg} COMMAND ${target} ${arg})
    set_tests_properties(Comp${arg}
                         PROPERTIES PASS_REGULAR_EXPRESSION ${result})
endfunction()

do_test(Tutorial 4 "4 is 2")
do_test(Tutorial 9 "9 is 3")
do_test(Tutorial 5 "5 is 2.236")
do_test(Tutorial 7 "7 is 2.645")
do_test(Tutorial 25 "25 is 5")
do_test(Tutorial -25 "-25 is (-nan|nan|0)")
do_test(Tutorial 0.0001 "0.0001 is 0.01")
```

参考：[Step 5: Installing and Testing (Exercise 2 - Testing Support) | cmake.org](https://cmake.org/cmake/help/latest/guide/tutorial/Installing%20and%20Testing.html#exercise-2-testing-support)

### 检查依赖可用性

某些 API 在特定的平台上可能不可用。为了检测这些 API 能否在当前平台正常使用，我们可以让 CMake 先编译一段测试代码，如果编译通过，再继续编译过程。

```cmake
# 引入模块 CheckCXXSourceCompiles
include(CheckCXXSourceCompiles)

# 编译测试代码，若通过则设置 HAVE_LOG 变量
check_cxx_source_compiles("
        #include <cmath>
        int main() {
            std::log(1.0);
            return 0;
        }
    " HAVE_LOG)

# 编译测试代码，若通过则设置 HAVE_EXP 变量
check_cxx_source_compiles("
        #include <cmath>
        int main() {
            std::exp(1.0);
            return 0;
        }
    " HAVE_EXP)

# 若设置了 HAVE_LOG 和 HAVE_EXP 变量，则定义相应的宏
if(HAVE_LOG AND HAVE_EXP)
    target_compile_definitions(SqrtLibrary
                               PRIVATE "HAVE_LOG" "HAVE_EXP")
    target_link_libraries(MathFunctions PRIVATE SqrtLibrary)
endif()
```

mysqrt.cpp

```cpp
#include <cmath>

namespace mathfunctions {
namespace detail {
double mysqrt(double x)
    #if defined(HAVE_LOG) && defined(HAVE_EXP)
    double result = std::exp(std::log(x) * 0.5);
    std::cout << "Computing sqrt of " << x << " to be " << result << " using log and exp" << std::endl;
    #else
    // 不使用 exp() 和 log() 的实现……
    #endif
}
```

- [CheckCXXSourceCompiles](https://cmake.org/cmake/help/latest/module/CheckCXXSourceCompiles.html#module:CheckCXXSourceCompiles)
- [target_compile_definitions()](https://cmake.org/cmake/help/latest/command/target_compile_definitions.html#command:target_compile_definitions)

参考：[Step 7: Adding System Introspection (Exercise 1 - Assessing Dependency Availability) | cmake.org](https://cmake.org/cmake/help/latest/guide/tutorial/Adding%20System%20Introspection.html#exercise-1-assessing-dependency-availability)

### 在构建时执行自定义命令

MakeTable.cmake

```cmake
add_executable(MakeTable MakeTable.cxx)

target_link_libraries(MakeTable PRIVATE tutorial_compiler_flags)

add_custom_command(
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/Table.h
    COMMAND MakeTable ${CMAKE_CURRENT_BINARY_DIR}/Table.h
    DEPENDS MakeTable)
```

CMakeLists.txt

```cmake
include(MakeTable.cmake)

add_library(SqrtLibrary STATIC
            mysqrt.cxx
            ${CMAKE_CURRENT_BINARY_DIR}/Table.h)

target_include_directories(SqrtLibrary PRIVATE
                           ${CMAKE_CURRENT_BINARY_DIR})
```

mysqrt.cpp

```cpp
double mysqrt(double x) {
    if (x <= 0) {
        return 0;
    }

  // use the table to help find an initial value
  double result = x;
  if (x >= 1 && x < 10) {
      std::cout << "Use the table to help find an initial value " << std::endl;
    result = sqrtTable[static_cast<int>(x)];
  }

  // do ten iterations
  for (int i = 0; i < 10; ++i) {
    if (result <= 0) {
      result = 0.1;
    }
    double delta = x - (result * result);
    result = result + 0.5 * delta / result;
    std::cout << "Computing sqrt of " << x << " to be " << result << std::endl;
  }

  return result;
}
}
}
```

参考：[Step 8: Adding a Custom Command and Generated File | cmake.org](https://cmake.org/cmake/help/latest/guide/tutorial/Adding%20a%20Custom%20Command%20and%20Generated%20File.html)

### 打包安装程序

CMakeLists.txt

```cmake
# 引入 InstallRequiredSystemLibraries 包
include(InstallRequiredSystemLibraries)

# 设置 CPack 参数
set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/License.txt")
set(CPACK_PACKAGE_VERSION_MAJOR "${Tutorial_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${Tutorial_VERSION_MINOR}")
set(CPACK_GENERATOR "TGZ")
set(CPACK_SOURCE_GENERATOR "TGZ")

# 引入 CPack 包
include(CPack)
```

构建项目，然后在二进制目录中打包：

```sh
cpack
```

参考：[Step 9: Packaging an Installer | cmake.org](https://cmake.org/cmake/help/latest/guide/tutorial/Packaging%20an%20Installer.html)

### 选择静态库或动态库

```cmake
# 定义布尔选项
option(BUILD_SHARED_LIBS "Build using shared libraries" ON)

# 设置输出目录
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}")

# 声明 SqrtLibrary 在默认情况下需要位置无关码共享库
set_target_properties(SqrtLibrary PROPERTIES
                      POSITION_INDEPENDENT_CODE ${BUILD_SHARED_LIBS})

# 在 Windows 上构建时使用 declspec(dllexport)
target_compile_definitions(MathFunctions PRIVATE "EXPORTING_MYMATH")
```

MathFunctions.h

```cpp
#if defined(_WIN32)  // Windows 平台
    #if defined(EXPORTING_MYMATH)
        #define DECLSPEC __declspec(dllexport)
    #else
        #define DECLSPEC __declspec(dllimport)
    #endif
#else  // 非 Windows 平台
    #define DECLSPEC
#endif

namespace mathfunctions {
double DECLSPEC sqrt(double x);
}
```

参考：[Step 10: Selecting Static or Shared Libraries | cmake.org](https://cmake.org/cmake/help/latest/guide/tutorial/Selecting%20Static%20or%20Shared%20Libraries.html)

## Troubleshooting

### 找不到动态链接库

Linux 上的动态库路径变量是 `LD_LIBRARY_PATH`，而 macOS 上的动态库路径变量是 `DYLD_LIBRARY_PATH`

在 CMakeLists.txt 中指定动态库路径：[linux 下通过 rpath 解决 cmake 动态编译后找不到动态链接库问题 | 稀土掘金](https://juejin.cn/post/6936408961050476558/)

[RPATH 简介以及 CMake 中的处理 | blog.xizhibei.me](https://blog.xizhibei.me/2021/02/12/a-brief-intro-of-rpath/)
我在 macOS 上使用时没有用到 `$ORIGIN` 变量，而是直接指定了动态库的路径。

---

参考：[CMake Tutorial | cmake.org](https://cmake.org/cmake/help/latest/guide/tutorial/index.html)