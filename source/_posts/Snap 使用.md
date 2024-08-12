Snap 是一个或多个应用程序的捆绑包，可在许多不同的 Linux 发行版中使用，无需依赖或修改。Snap 可从 [Snap Store](https://snapcraft.io/)（一个拥有数百万用户的公共应用程序商店）中发现和安装。很多常用的软件，如 [VS Code](https://snapcraft.io/code) 和 [Spotify](https://snapcraft.io/spotify) 都可以在 Snap Store 上找到。

具体来说，Snap Store 相当于 Linux 上的 [App Store](https://zh.wikipedia.org/wiki/App_Store_(iOS/iPadOS))。

## 常用命令

```sh
snap find "media player"  # 搜索 snap
snap info <snap>          # 查看 snap 详细信息
snap list                 # 列出已安装 snap
sudo snap refresh         # 更新全部 snap
sudo snap remove <snap>   # 卸载 snap
sudo snap install <snap>  # 安装 snap
sudo snap install --channel=edge <snap>   # 安装时指定 channel
sudo snap switch --channel=stable <snap>  # 切换 channel
```

大部分 snap 应用程序可以从命令行或者桌面启动器运行。如果不起作用，尝试使用 `snap run` 命令：

```sh
snap run <snap>
```

Snap 应用程序链接位于 `/snap/bin`，已经添加到系统 $PATH。

参考：[Quickstart tour | Snapcraft](https://snapcraft.io/docs/quickstart-tour)

## 已知问题

**1Password 浏览器插件通信问题**

Ubuntu 已经将系统自带的 Firefox 全部改由 Snap 安装。然而 Snap 的沙盒机制会导致 Snap 版 Firefox 无法与外界程序通信。这导致 1Password 浏览器插件无法连接到 1Password 桌面应用。反过来 Snap 版 1Password 桌面应用也会因为沙盒机制而无法与 1Password 浏览器插件通信。相关解释：[If the 1Password browser extension doesn't unlock when you unlock the 1Password app | 1Password Support](https://support.1password.com/connect-1password-browser-app/#if-youre-using-a-linux-computer)

**Firefox 无法上传文件**

沙盒机制也会导致 Snap 版的 Firefox 无法从计算机上传文件。

因此，建议 1Password 和 Firefox 都不要安装 Snap 版本。安装普通版 Firefox 的方法参考[使用基于 Debian 发行版的 .deb 包安装 Firefox | Mozilla Support](https://support.mozilla.org/zh-CN/kb/install-firefox-linux#w_shi-yong-ji-yu-debian-fa-xing-ban-de-deb-bao-an-zhuang-firefox)

我个人不太喜欢使用 Snap 包，最喜欢的还是使用 DEB 包安装程序。