苹果于 2020 年推出了自家设计的基于 ARM 架构的 M1 芯片，在日常生活的大部分使用过程中，M1 的体验很好。然而，依然存在一小部分软件无法兼容 ARM 架构，需要我们模拟 x86 的架构来运行。

其中一个例子是 [Kettle](https://github.com/pentaho/pentaho-kettle?tab=readme-ov-file)（又称 PDI）。笔者当年使用 Kettle 时无法直接双击打开，为了打开 Kettle，我们需要进入 x86 模式的终端，然后再在 x86 终端中输入命令打开 **x86 版本的 Kettle**。

> 截至 2022.10.13，Pentaho 9.3 尚不支持 AArch64 架构，必须在 x86_64 模式的终端中运行，并且依赖 **x86_64** 的 Java 11 运行环境。

现在已经很久没用过 Kettle 了，不过当时为了打开 Kettle 还是费了不少事，所以今天想起来特此记录一下。

## 打开 x86 模式的终端

首先我们要打开一个运行在 x86 模式下的终端。如何做呢？使用下面这条命令即可打开一个 x86 模式的终端：

```sh
env /usr/bin/arch -x86_64 /bin/zsh --login
```

命令各部分的解释：

1. `env`: 用于在重建后的环境中运行命令。
2. `/usr/bin/arch -x86_64`: 这部分调用 `arch` 命令（通常是由 Rosetta 2 提供，在一些如苹果的 M1/M2 芯片的 ARM 架构计算机上用于模拟 x86_64 架构的环境），并指定使用 `-x86_64` 选项，最终目的是模拟一个 x86_64 架构的环境。
3. `/bin/zsh`: 这是 Zsh shell 的路径。Zsh 是一个常用的命令行解释器。
4. `--login`: 这是传递给 Zsh 的参数，指示它启动一个登录 shell。

因此，当你运行这条命令时，它会在模拟的 x86_64 环境中启动一个新的 Zsh 登录 shell。

### 设置终端描述文件方便下次使用（可选）

为了我们每次启动 x86 终端时不用再输入这条命令，我们可以为终端创建一个描述文件，专门用来打开 x86 终端。

打开终端，按下 <kbd>⌘</kbd> + <kbd>,</kbd> 打开设置，转到“描述文件”标签页，在窗口的左下角点击 `+` 号创建一个新描述文件。

在新描述文件的设置窗口中，转到“Shell”标签页，勾选“运行命令”复选框，并在命令栏中将我们的命令 `env /usr/bin/arch -x86_64 /bin/zsh --login` 输入进去。

之后，你可以为你的描述文件设置一个新的名字，比如 `x86 终端`，我这里设置成了 `Rosetta Shell`。

设置完成的效果如图：

![image](https://s2.loli.net/2024/07/07/riIzJgkhPYWVNa6.png)

下次你要打开 x86 终端的时候，直接选择启动你的描述文件就可以打开一个运行在 x86 模式下的终端了。或者，如果你经常要使用 x86 终端的话，你可以将你的描述文件设为默认（点击左下角的“默认”按钮）。

## 安装 x86 版本的 Homebrew

为了下载到 x86 版本的 Kettle，我们首先需要一个 x86 版本的 Homebrew，这样一会儿使用 `brew` 命令下载到的 Kettle 才是 x86 版本的。

在 x86 终端中运行 Homebrew 安装命令：

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

> 由于国内的网络原因，下载 Homebrew 的速度可能较慢。对于还没有实现高速科技上网的同学，这里提供一个网站 [Homebrew 中文网](https://brew.idayer.com/)，可以按照这个网站提供的链接快速安装 Homebrew。

⚠️ 注意，有些同学可能已经安装过 ARM 架构的 Homebrew 了。为了让两种 Homebrew 共存，你还需要在你的 `~/.zshrc` 文件中添加如下命令：

```sh
# 如果运行在转译模式下，将 x86 架构 Homebrew 的路径添加到环境变量 PATH
if [ "$(sysctl -n sysctl.proc_translated)" = "1" ]; then
    PATH="/usr/local/homebrew/bin:/usr/local/homebrew/sbin:$PATH"
fi
```

安装完成之后，使用下面的命令验证你是否正在使用 x86 架构的 Homebrew（你可能需要启动一个新的 x86 终端）：

```sh
which brew
```

如果命令的输出是 `/usr/local/homebrew/bin/brew`，则说明你的 Homebrew 是 x86 架构的。如果输出是 `/opt/homebrew/bin/brew`，则说明你的 Homebrew 是 ARM 架构的。

## 安装 x86 版本的 Kettle

确定当前使用的是 x86 架构的 Homebrew后，使用下面的命令安装 x86 架构的 Kettle：

```sh
brew install kettle
```

安装完成之后，为了方便使用，我们可以在 `~/.zshrc` 中添加如下命令：

```sh
alias kettle='export JAVA_HOME="$(/usr/libexec/java_home -v 11)" && PATH="${JAVA_HOME}/bin:$PATH" && env /usr/bin/arch -x86_64 /bin/sh /usr/local/homebrew/Cellar/kettle/9.3.0.0-428/libexec/spoon.sh'
```

> Kettle 的版本号 `9.3.0.0-428` 需要你根据自己的情况修改。你可以通过命令 `brew info kettle` 查看自己安装的 Kettle 的版本号。

解释一下这条命令：

这里设置了一个别名 `kettle`，别名的作用就是把一长串命令用一个短的名字替代，下次我们想执行这条命令时只需键入这条短的名字就可以了。

接下来看别名的组成部分：

```sh
export JAVA_HOME="$(/usr/libexec/java_home -v 11)" && PATH="${JAVA_HOME}/bin:$PATH" && env /usr/bin/arch -x86_64 /bin/sh /usr/local/homebrew/Cellar/kettle/9.3.0.0-428/libexec/spoon.sh
```

首先设置了一个环境变量 `JAVA_HOME`，并把 `JAVA_HOME` 添加到环境变量 `PATH` 中。

这里的 `JAVA_HOME` 使用了 Java 11 的根目录`$(/usr/libexec/java_home -v 11)`。因为 Kettle 是基于 Java 的，而 Kettle 9.3 兼容最好的 Java 版本是 Java 11。如果你还没有安装 Java 11 的话，在 x86 终端中运行 `brew install openjdk@11`。

接下来，使用了类似前面的命令 `env /usr/bin/arch -x86_64 /bin/sh` 在  x86 模式下打开 `sh`，并使用 `sh` 启动 Spoon `/usr/local/homebrew/Cellar/kettle/9.3.0.0-428/libexec/spoon.sh`

## 安装 JDK 11

笔者当时安装 Kettle 时的最新版是 9.3，该版本的 Kettle 支持的 Java 版本为 Java 11。因此我们要安装一个 JDK 11。由于我们的 JDK 是要为 x86 版本的 Kettle 服务的，因此我们也要安装 x86 版本的 JDK。

确保你在 x86 模式的终端，然后运行：

```sh
brew install openjdk@11
```

## 启动 Kettle

现在，如果你想打开 Kettle，只需在终端输入 `kettle` 即可（不用打开 x86 终端）：

```sh
kettle
```

参考：

- [关于在 M1 Mac 上安装部署 PDI (kettle)](https://blog.csdn.net/ColeMEI/article/details/118616719)
- [Pentaho 9.3 支持 Java 11](https://help.hitachivantara.com/Documentation/Pentaho/9.3/What's_new_in_Pentaho_9.3)
- [pentaho-kettle | GitHub](https://github.com/pentaho/pentaho-kettle)