## 安装

1. 安装苹果命令行开发工具:

   ```sh
   xcode-select --install
   ```

2. 下载并打开 [MacPorts 安装包](https://github.com/macports/macports-base/releases/)。

## 使用

MacPorts 中的软件包称为 port。

```sh
sudo port selfupdate       # 更新 MacPorts
sudo port install <port>   # 安装 port
sudo port uninstall <port> # 卸载 port
sudo port upgrade outdated # 更新所有已安装的 port
port search <port>         # 搜索 port
port info <port>           # 查看 port 信息
port help                  # 获取帮助
```

## 卸载

```sh
sudo port upgrade outdated <port> # 卸载所有已安装的 port

# 删除 macports 用户和组
sudo dscl . -delete /Users/macports
sudo dscl . -delete /Groups/macports

# 移除 MacPorts 的其他部分
sudo rm -rf \
    /opt/local \
    /Applications/DarwinPorts \
    /Applications/MacPorts \
    /Library/LaunchDaemons/org.macports.* \
    /Library/Receipts/DarwinPorts*.pkg \
    /Library/Receipts/MacPorts*.pkg \
    /Library/StartupItems/DarwinPortsStartup \
    /Library/Tcl/darwinports1.0 \
    /Library/Tcl/macports1.0 \
    ~/.macports
```

参考：

- [The MacPorts Project - Installing MacPorts](https://www.macports.org/install.php)
- [The MacPorts Project - Available Ports](https://ports.macports.org/)
- [MacPorts Guide](https://guide.macports.org/#using)