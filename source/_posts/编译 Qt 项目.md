参考：[Qt 编程指南](https://qtguide.ustclug.org/)

一个最小化工作示例：[qt-minimal | GitHub](https://github.com/Undefined443/qt-minimal)

## 源文件

main.cpp

```cpp
#include <QApplication>
#include <QLabel>

int main(int argc, char *argv[]) {
    QApplication app(argc, argv);

    QLabel label(QLabel::tr("Hello, Qt!"));
    label.resize(200, 30);
    label.setWindowTitle("First Qt App");
    label.show();

    return app.exec();
}
```

## 使用 QMake 构建 Qt 项目

创建一个 Qt 项目文件 (`.pro` 文件) ：

```sh
qmake -project "QT+=widgets"
```

> 从 Qt 5 开始，QtWidgets 模块被从 QtGui 模块中分离且不会自动添加，因此这里手动添加 QtWidgets 模块。

该命令将扫描当前目录中的所有文件，然后生成对应的 `.pro` 文件。或者也可以手动创建一个：

MyQtProject.pro

```pro
QT += widgets

SOURCES += main.cpp

# 如果你有头文件或 UI 文件，也可以添加到这里。
# HEADERS += mainwindow.h
# FORMS += mainwindow.ui
```

通过 `.pro` 文件生成 Makefile：

```sh
qmake
```

通过 Makefile 编译项目：

```sh
make
```

参见：

- [Creating Project Files | Qt Docs](https://doc.qt.io/qt-6/qmake-project-files.html)
- [qmake Manual | Qt Docs](https://doc.qt.io/qt-6/qmake-manual.html)

## 使用 CMake 构建 Qt 项目

创建 `CMakeLists.txt` 文件：

```cmake
cmake_minimum_required(VERSION 3.16)

project(MyQtProject VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Qt6 REQUIRED COMPONENTS Widgets)

qt_add_executable(MyQtProject main.cpp)

target_link_libraries(MyQtProject PRIVATE Qt6::Widgets)

set_target_properties(MyQtProject PROPERTIES
    WIN32_EXECUTABLE ON
    MACOSX_BUNDLE ON
)
```

生成和构建项目：

```sh
mkdir build && cd build
cmake ..
make
```

最终将生成 `MyQtProject` 可执行文件。

参见：

- [Building a C++ GUI application | Qt Docs](https://doc.qt.io/qt-6/cmake-get-started.html#building-a-c-gui-application)
- [Qt 构建c make 工程方法总结 | 博客园](https://www.cnblogs.com/learnhow/p/15110705.html)
- [CMake 使用 | 博客园](https://www.cnblogs.com/Undefined443/p/-/cmake)

## 部署

使用 QMake 或 CMake 构建的 Qt 程序使用了 Qt 的动态链接库。为了使程序在没有安装 Qt 动态链接库的机器上也能运行，我们需要使用部署工具将这些动态链接库和程序一起打包发布。

### Windows

[CMake -static 编译 | CSDN](https://blog.csdn.net/qq_19982783/article/details/77740703)

```cmd
windeployqt MyQtProject.exe
```

在 Qt 6.5.0 上使用 windeployqt 会出现如下错误：

```
Cannot open .: Access is denied.
```

解决办法：

```cmd
windeployqt --no-translations YOUR_QT_APP.exe
```

### macOS

```sh
macdeployqt MyQtProject.app
```

[Qt 项目在 MacOS 平台上面发布成可执行程序 .app | ifmet.cn](https://ifmet.cn/posts/6f46465b/)

## Troubleshooting

### Could not find the Qt platform plugin "cocoa" in ""

解决方法：添加环境变量 `QT_QPA_PLATFORM_PLUGIN_PATH`

```sh
export QT_QPA_PLATFORM_PLUGIN_PATH="/opt/homebrew/opt/qt/share/qt/plugins"
```

参考：[Qt could not find the platform plugin cocoa | Stack Overflow](https://stackoverflow.com/questions/54297627/qt-could-not-find-the-platform-plugin-cocoa)