## 使用

```sh
brew install
brew uninstall|remove|rm
brew list           # *显示已安装软件列表
brew upgrade        #  更新 Homebrew
brew search         # *搜索软件
brew info           # *显示软件详细信息
brew help [COMMAND] #  显示命令帮助
brew tap
brew tap-info
man brew            #  显示帮助手册
```

## Formulae 和 Cask 的区别

一般情况下，Formulae 是命令行程序，Cask 是图形程序。

Homebrew Cask 项目：原先是独立于 Homebrew 的一个扩展，提供对以二进制形式发布的 macOS 应用的管理，但现在与 Homebrew 密切合作。

Formulae 和 Cask：Homebrew 将自己的包定义文件称为 Formulae，而 Homebrew Cask 将它们称为 Cask。Cask 和 Formulae 一样，是用基于 Ruby 的 DSL 编写的文件，描述如何安装软件。

[What is the difference between brew install xxx and brew cask install xxx | Stackoverflow](https://stackoverflow.com/questions/46403937/what-is-the-difference-between-brew-install-xxx-and-brew-cask-install-xxx)

## 换源

### 使用镜像源

一些重要的变动：

- [`4.0.0`](https://brew.sh/2023/02/16/homebrew-4.0.0/)：软件包信息不再从 `homebrew/core` 和 `homebrew/cask` 获取，转而使用从 API (formulae.brew.sh) 获取的信息指导软件包安装。相应地，弃用了环境变量 `HOMEBREW_CORE_GIT_REMOTE` 而启用了环境变量 `HOMEBREW_API_DOMAIN`。
- [`4.3.0`](https://brew.sh/2024/05/14/homebrew-4.3.0/)：弃用了 `homebrew/cask-fonts` 和 `homebrew/cask-versions`，移入了 `homebrew/cask`。

设置环境变量，在你的 `.zshrc`/`.bashrc` 中添加以下内容：

```sh
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git" # 指定 Homebrew 自身的 Git 仓库的镜像地址
brew update # 更新索引
```

```sh
export HOMEBREW_API_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles/api" # 指定 Homebrew 的 API 域名
export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles" # 指定 Homebrew 的预编译二进制包的下载域名
export HOMEBREW_PIP_INDEX_URL="https://mirrors.ustc.edu.cn/pypi/web/simple" # 指定 Homebrew 中使用的 Python 包管理器 pip 的索引 URL
```

接下来在终端中运行如下命令：

```sh
# 指定 tap 仓库的 Git 远程地址
brew tap --custom-remote --force-auto-update "homebrew/command-not-found" "https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-command-not-found.git"
brew tap --custom-remote --force-auto-update "homebrew/services" "https://mirrors.ustc.edu.cn/homebrew-services.git"
```

> 我这里科大源的下载速度比较快

### 恢复为官方源

```sh
export HOMEBREW_BREW_GIT_REMOTE="https://github.com/Homebrew/brew.git" # 指定 Homebrew 自身的 Git 仓库的镜像地址
brew update # 更新索引
```

在你的 `.zshrc`/`.bashrc` 中删除以下内容：

```sh
export HOMEBREW_API_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles/api" # 指定 Homebrew 的 API 域名
export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles" # 指定 Homebrew 的预编译二进制包的下载域名
export HOMEBREW_PIP_INDEX_URL="https://mirrors.ustc.edu.cn/pypi/web/simple" # 指定 Homebrew 中使用的 Python 包管理器 pip 的索引 URL
```

接下来在终端中运行如下命令：

```sh
# 恢复 tap 仓库的 Git 远程地址
brew tap --custom-remote "homebrew/command-not-found" "https://github.com/Homebrew/homebrew-command-not-found.git"
brew tap --custom-remote "homebrew/services" "https://github.com/Homebrew/homebrew-services.git"
```

### 使用镜像源安装 Homebrew

如果你还没有安装 Homebrew，你可以使用下面的命令从镜像源安装 Homebrew：

```sh
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git"
export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles"
export HOMEBREW_API_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles/api"
/bin/bash -c "$(curl -fsSL https://mirrors.ustc.edu.cn/misc/brew-install.sh)"
```

### 附录：各镜像站参考文档

- [Homebrew 源使用帮助 | 中国科学技术大学开源软件镜像](https://mirrors.ustc.edu.cn/help/brew.git.html)
- [Homebrew / Linuxbrew 镜像使用帮助 | 清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/)
- [Homebrew 镜像 | 阿里巴巴开源镜像站](https://developer.aliyun.com/mirror/homebrew)
- [Homebrew 镜像使用帮助 | 腾讯软件源](https://mirrors.cloud.tencent.com/help/homebrew.html)
- [Homebrew Mirror | 南方科技大学开源镜像站](https://mirrors.sustech.edu.cn/help/homebrew.html)
- [Homebrew 中文网](https://brew.idayer.com/guide/m1/#%E8%AE%BE%E7%BD%AE%E9%95%9C%E5%83%8F)

## tap

`tap` 是 Homebrew 的一个扩展机制，可以让用户添加第三方仓库，从而安装第三方仓库中的软件。

`brew tap`：用于添加第三方仓库。

```sh
brew tap  # 查看已添加的仓库
brew tap owner/repo  # 添加仓库 owner/homebrew-repo
brew untap owner/repo  # 删除仓库
```

也可以不 tap 仓库直接使用仓库中的 Cask:

```sh
brew install owner/repo/package
```

## 安装路径

对于使用 Apple Silicon 芯片的 macOS，下载好的包会放在 `/opt/homebrew/Cellar/` 目录下，并且会链接到 `/opt/homebrew/opt/` 目录中。

## 一些名词翻译

- Caveats：注意事项