## 简介

[MinGW-w64](https://zh.wikipedia.org/wiki/MinGW-w64) 是 [MinGW](https://zh.wikipedia.org/wiki/MinGW) 项目的 64 位版本。MinGW（Minimalist GNU for Windows）是 [GCC](https://zh.wikipedia.org/wiki/GCC) 编译套件和 [GNU Binutils](https://zh.wikipedia.org/wiki/GNU_Binutils) 移植到 Windows 下的产物。简单理解，它就是 Windows 平台上的 GCC。

MinGW-w64 项目官网：[www.mingw-w64.org](https://www.mingw-w64.org/)

由于 MinGW-w64 项目只提供源代码而不提供编译好的二进制文件，因此我们要寻找的所谓“Windows 平台上的 GCC”实际上是提供二进制文件的 [MinGW-W64-binaries](https://github.com/niXman/mingw-builds-binaries/) 项目。很多人直接用搜索引擎搜索“MinGW-w64”，结果被引到 MinGW-w64 项目的 [Source Forge 代码仓库](https://sourceforge.net/projects/mingw-w64/)，下载下来一堆源代码，不知道怎么用，实际上是找错了项目。

## 安装 MinGW-w64

MinGW-w64 的正确安装方式是：

1. 下载 MinGW-w64 压缩包并解压。

   打开 MinGW-W64-binaries 项目的 [Releases](https://github.com/niXman/mingw-builds-binaries/releases) 页面，找到最新（Latest）的 Release。

   MinGW-W64-binaries 项目根据不同的编译选项提供了不同的二进制压缩包。一般来说，最适合我们的选项是 `x86_64-x.x.x-release-posix-seh-ucrt-rt_v11-rev0.7z
`（`x.x.x` 为版本号）。

   下载该压缩包，解压到合适的位置，比如 `C:\mingw64`。

3. 添加 `Path` 环境变量。

   `PATH` 是系统查找二进制（可执行）文件时使用的路径，设置了 `PATH`，就能让系统以及其他程序找到 MinGW-w64 的可执行文件。

   打开 `设置` > `系统` > `关于` > `高级系统设置`，在 `高级` 选项卡下打开 `环境变量`，你会发现 `xxx 的用户变量` 和 `系统变量` 栏中都有一个 `Path` 变量。这两个 `Path` 编辑哪个都行，区别是用户变量只对自己可见，系统变量则对系统上的所有用户都可见。我们现在电脑都是自己一个人用，所以对我们来说没区别。

   打开“编辑环境变量”窗口，点“新建”，将你刚刚解压的 MinGW-w64 压缩包内 `bin` 目录的路径填进来，比如 `C:\mingw64\bin`。

   ![image](https://img2024.cnblogs.com/blog/2778973/202408/2778973-20240801162234363-333834556.png)

   点确定关闭刚刚打开的各个窗口，然后重启电脑。

4. 验证安装。

   打开终端，输入 `gcc --version`，如果你的输出和我的类似，则证明你安装成功了。

   ```powershell
   $ gcc --version
   gcc.exe (x86_64-posix-seh-rev0, Built by MinGW-Builds project) 13.2.0
   Copyright (C) 2023 Free Software Foundation, Inc.
   This is free software; see the source for copying conditions.  There is NO
   warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
   ```

参考：

- [What is the difference between MinGW, MinGW-w64 and MinGW-builds? | Stack Overflow](https://stackoverflow.com/questions/25582110/what-is-the-difference-between-mingw-mingw-w64-and-mingw-builds)

- [Meaning of options in mingw-w64 installer | Stack Overflow](https://stackoverflow.com/questions/29947302/meaning-of-options-in-mingw-w64-installer)

- [mingw-w64 threads: posix vs win32 | Stack Overflow](https://stackoverflow.com/questions/17242516/mingw-w64-threads-posix-vs-win32)