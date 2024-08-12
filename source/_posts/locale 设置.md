## locale 介绍

在计算机系统的终端（或命令行）中，locale（本地化）设置是指与本地语言、国家和文化偏好有关的环境变量的配置。这些设定决定了程序如何处理和显示字符、时间、日期格式、货币等。

在类 Unix 系统（比如 Linux 和 macOS）中，locale 设置由一系列环境变量来定义，这些环境变量包括：

- `LANG`: 这个环境变量用来设定默认的系统语言和字符集。
- `LANGUAGE`: 用于设置在显示消息时使用的语言排列顺序。
- `LC_CTYPE`: 用于设置字符分类和字符串处理，以确定字符类型。
- `LC_NUMERIC`: 用于设置数字格式，例如小数点字符和千位分隔符。
- `LC_TIME`: 用于设置本地格式化时间和日期字符串。
- `LC_COLLATE`: 定义排序规则，用于比较字符串。
- `LC_MONETARY`: 用于货币格式的本地化设置。
- `LC_MESSAGES`: 用于本地化系统和应用程序输出的消息。
- `LC_PAPER`: 用于设置纸张尺寸的本地化信息。
- `LC_NAME`: 用于名称表示的本地化信息。
- `LC_ADDRESS`: 用于地址、电话号码和邮编表示的本地化信息。
- `LC_TELEPHONE`: 用于电话号码格式的本地化信息。
- `LC_MEASUREMENT`: 用来定义度量单位的信息。
- `LC_IDENTIFICATION`: 含有本地化设置自身的描述信息。

如果你想查看当前系统中的 locale 设置，可以在终端中使用 `locale` 命令。例如，要查看当前所有可用的 locale，可以运行 `locale -a`：

```sh
$ locale -a
en_NZ
en_US.US-ASCII
en_US.UTF-8
en_NZ.ISO8859-1
en_AU.US-ASCII
en_US
en_NZ.UTF-8
en_AU.ISO8859-15
en_US.ISO8859-15
en_NZ.ISO8859-15
en_AU.UTF-8
en_CA
en_NZ.US-ASCII
en_GB.ISO8859-1
en_CA.US-ASCII
en_CA.ISO8859-15
en_US.ISO8859-1
en_GB.UTF-8
en_GB.US-ASCII
en_AU
en_GB
en_CA.UTF-8
en_IE.UTF-8
en_CA.ISO8859-1
en_AU.ISO8859-1
en_IE
en_GB.ISO8859-15
...
zh_CN.UTF-8
zh_HK
zh_TW
zh_CN.GB2312
zh_CN.GBK
zh_CN.GB18030
zh_CN
zh_TW.Big5
zh_TW.UTF-8
zh_CN.eucCN
zh_HK.UTF-8
zh_HK.Big5HKSCS
```

要查看当前设置，只需运行 `locale`：

```sh
$ locale
LANG="zh_CN.UTF-8"
LC_COLLATE="zh_CN.UTF-8"
LC_CTYPE="zh_CN.UTF-8"
LC_MESSAGES="zh_CN.UTF-8"
LC_MONETARY="zh_CN.UTF-8"
LC_NUMERIC="zh_CN.UTF-8"
LC_TIME="zh_CN.UTF-8"
LC_ALL=
```

这将显示所有的 locale 相关环境变量及其当前值。

更改 locale 通常涉及到编辑一些系统文件（如 `~/.bashrc`、`~/.bash_profile`、`/etc/locale.conf`，取决于你的系统和 shell），或者使用如 `update-locale`、`dpkg-reconfigure locales` 等命令行工具。更改后，可能需要重新登录或者重新启动系统来使变更生效。

## PS

一般来说 `LC_MESSAGES` 控制命令行工具的输出语言。如果你想要更改命令行工具的输出语言，只需设置 `LC_MESSAGES` 环境变量即可。

比如：

zh_CN

```sh
$ LC_MESSAGES="zh_CN.UTF-8" git pull
已经是最新的。
```

en_US

```sh
$ LC_MESSAGES="en_US.UTF-8" git pull
Already up to date.
```
