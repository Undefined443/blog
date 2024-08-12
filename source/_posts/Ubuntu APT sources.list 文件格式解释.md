## 单行风格（传统）

传统的 `sourses.list` 文件使用单行风格配置，下面是两条单行风格的配置项：

```list
deb http://archive.ubuntu.com/ubuntu jammy main restricted
deb [arch=amd64 signed-by=/usr/share/keyrings/paperspace.asc] https://metadata.paperspace.com/v2/apt paperspace main
```

配置的格式如下：

```list
deb [ option1=value1 option2=value2 ] uri suite [component1] [component2] [...]
```

- 第一列：软件源类型。`deb` 表示该软件源包含二进制软件包，与之对应的是 `deb-src`，表示源代码包。
- 第二列：第二列是可选的，表示软件源的配置项。一般会配置软件源的架构类型和 GPG 公钥位置
- 第三列：软件源的 URI
- 第四列：软件源套件。对于 Ubuntu 源来说这里应该填 [Ubuntu 版本代号](https://wiki.ubuntu.com/Releases)
- 第五列及之后：组件，对于 Ubuntu 源来说这里一般包含如下组件类型：
   - `main`: 受官方支持的自由软件
   - `restricted`: 受官方支持的非自由软件
   - `universe`: 不受官方支持的自由软件
   - `multiverse`: 不受官方支持的非自由软件

另外，这样的一行：

```list
deb http://archive.ubuntu.com/ubuntu jammy main restricted
```

等于这样的两行：

```list
deb http://archive.ubuntu.com/ubuntu jammy main
deb http://archive.ubuntu.com/ubuntu jammy restricted
```

如果你的 `sources.list` 提示配置出现了重复项，请根据上面的规则删除重复的条目。

## DEB822 风格

APT 正在推行一种新的格式，名为 DEB822 风格配置，Ubuntu 24.04 就使用了这种格式。使用新格式的配置文件位于 `/etc/apt/sources.list.d/ubuntu.sources` 。下面是一条 DEB822 风格配置项：

```sources
Types: deb
URIs: http://security.ubuntu.com/ubuntu
Suites: noble-security
Components: main universe restricted multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
```

配置的格式如下：

```sources
Types: deb deb-src
URIs: uri
Suites: suite
Components: [component1] [component2] [...]
option1: value1
option2: value2
```

关于 `sourses.list` 文件的更多解释请参考帮助手册 `man 5 sources.list` 或者查看在线帮助手册 [sources.list(5)](https://manpages.ubuntu.com/manpages/jammy/man5/sources.list.5.html)

参考：

- [What is the format of the Ubuntu sources.list file? | askUbuntu](https://askubuntu.com/questions/737498/what-is-the-format-of-the-ubuntu-sources-list-file)
- [Whats the difference between multiverse universe restricted and main?](https://askubuntu.com/questions/58364/whats-the-difference-between-multiverse-universe-restricted-and-main)