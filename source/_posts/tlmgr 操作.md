### 宏包管理

```sh
sudo tlmgr install <package>  # 安装宏包
sudo tlmgr install scheme-full  # 安装全部宏包

sudo tlmgr remove <package>  # 卸载宏包
sudo tlmgr remove --all --force  # 卸载所有宏包（包括 TeX Live）

tlmgr update --list  # 列出所有可更新宏包
sudo tlmgr update --all --self  # 更新所有宏包（包括 TeX Live）
sudo tlmgr update --all --reinstall-forcibly-removed  # 当某些包更新失败时继续更新
```

### 添加链接文件

在 `/usr/local/bin` 中添加 TeX Live 二进制程序的链接文件。

```sh
tlmgr path add  # 添加链接文件
tlmgr path remove  # 移除链接文件
```

### 查找宏包

```sh
tlmgr search --global  # 在 TeX Live 数据库查找宏包
tlmgr info <package>  # 查看宏包信息
```

> 对于 TeX Live，命令前不需要加 `sudo`。对于 MacTeX，命令前需要加 `sudo`

### 帮助文档

```sh
tlmgr --help
```

参考：[full documentation for tlmgr](https://www.tug.org/texlive/doc/tlmgr.html)