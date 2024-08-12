我在使用 Homebrew 安装 Wireshark 的时候，Homebrew 要求我输入密码。此时我又不想安转 Wireshark 了，于是我没有输入密码并且按下了 Ctrl + C。后来，我又尝试安装 Wireshark，但此时 Homebrew 提示我已经安装了 Wireshark：

```sh
$ brew install wireshark --cask
Warning: Not upgrading wireshark, the latest version is already installed
```

于是我尝试卸载 Wireshark：

```sh
$ brew uninstall wireshark --cask
==> Uninstalling Cask wireshark
Error: Failure while executing; `/usr/bin/sudo -u root -E -- /usr/sbin/installer -pkg /opt/homebrew/Caskroom/wireshark/4.2.2/Uninstall\ ChmodBPF.pkg -target /` exited with 1. Here's the output:
installer: Package name is Uninstall ChmodBPF
installer: Installing at base path /
installer: The install failed. (安装器遇到了一个错误，导致安装失败。请联系软件生产企业以获得帮助。 运行软件包“Uninstall ChmodBPF.pkg”的脚本时出错。)
```

出现错误，无法卸载。

我通过手动删除 Caskroom 下的 `wireshark` 文件夹解决了错误：

```sh
rm -rf /opt/homebrew/Caskroom/wireshark/
```

此时再安装 Wireshark：

```sh
brew install wireshark --cask
```

即可完成安装。