C 语言中的多文件编程通常涉及将代码分散在几个不同的源文件（`.c` 文件）和头文件（`.h` 文件）中。这么做可以帮助你组织大型项目，提高代码的重用性，便于团队合作，分离接口和实现，以及加快编译时间。下面是一个多文件编程的基本步骤：

1. 创建头文件: 头文件通常包含结构体定义、全局变量声明、常量定义以及函数声明（也称为函数原型）。头文件通常有 `.h` 后缀。

   ```c
   /* file: myfunctions.h */
   #ifndef MYFUNCTIONS_H
   #define MYFUNCTIONS_H

   /* 函数声明 */
   void printHello(void);

   #endif /* MYFUNCTIONS_H */
   ```

2. 创建源文件: 源文件包含头文件的具体实现——函数的定义。

   ```c
   /* file: myfunctions.c */
   #include <stdio.h>
   #include "myfunctions.h"

   /* 函数定义 */
   void printHello(void){
       printf("Hello, world!\n");
   }
   ```

   注意，源文件应包括对应的头文件以及需要的标准库头文件。

3. 创建主源文件: 主源文件通常是包含 `main` 函数的 `.c` 文件，它会调用其他源文件中的函数。

   ```c
   /* file: main.c */
   #include "myfunctions.h"

   int main() {
       printHello();
       return 0;
   }
   ```

4. 编译代码: 当你有多个源文件时，你需要分别编译每个文件，然后将它们链接起来。如何执行这些取决于你使用的编译器。如果使用命令行编译器例如 GCC，你可以按照以下步骤操作：

   ```sh
   cc -c main.c         # 编译主源文件
   cc -c myfunctions.c  # 编译其他源文件
   cc -o myprogram main.o myfunctions.o  # 链接目标文件并生成可执行文件
   ```

   > 注意，你不需要编译头文件，因为头文件已经被其对应的源文件包含。

   `-c` 标志告诉编译器仅编译源文件，不进行链接。这会为每个源文件生成 `.o` 后缀的目标文件。最后，不带 `-c` 的 gcc 命令将链接所有目标文件生成最终的可执行文件。

5. 运行程序: 最后，你可以运行生成的可执行文件：

   ```sh
   ./program
   ```

   确保你的头文件中使用了预处理指令，如 `#ifndef`, `#define` 和 `#endif`，以防止头文件重复包含。

对于更大的项目而言，你可能需要使用一个构建系统或者高级的编译工具如 make 或 CMake，以简化编译过程。这样可以使用更复杂的构建规则，并自动处理文件之间的依赖关系。