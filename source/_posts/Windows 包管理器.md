## WinGet

WinGet 是微软官方的软件管理器。

[搜索 WinGet 包](https://winget.run/search)

### 常用命令

```powershell
winget install <packaeg>   # 安装包
winget uninstall <package> # 卸载包
winget list                # 列出已安装包
winget upgrade --all --include-unknown # 更新所有包
winget search <package>    # 查找包
winget show <package>      # 显示包详细信息
```

> 参考：[使用 winget 工具安装和管理应用程序 | Microsoft Docs](https://docs.microsoft.com/zh-cn/windows/package-manager/winget/)

### WinGet 换源

使用中科大源，在管理员模式下运行下面的命令：

```powershell
winget source remove winget
winget source add winget https://mirrors.ustc.edu.cn/winget-source
```

重置为官方源：

```powershell
winget source reset winget
```

参考：[WinGet | USTC Mirror Help](https://mirrors.ustc.edu.cn/help/winget-source.html)

## Chocolatey

安装：

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

> 参考：[Setup / Install | Chocolatey Docs](https://docs.chocolatey.org/en-us/choco/setup/)

使用：

```powershell
choco install <package>
```

## Scoop

- [Scoop 官网](https://scoop.sh)
- [Scoop 项目仓库](https://github.com/ScoopInstaller/Scoop)
- [Scoop 软件搜索](https://scoop.sh/#/apps)

安装：

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser # Optional: Needed to run a remote script the first time
irm get.scoop.sh | iex
```

使用：

```powershell
scoop bucket add extras      # 添加 extras 源
scoop install pasteboard     # 安装软件

scoop bucket add nerd-fonts  # 添加 nerd-fonts 源
scoop install sudo           # 安装 sudo
sudo scoop install -g Cascadia-Code  # 全局安装 Cascadia-Code

scoop bucket add versions
scoop install python27

scoop bucket add java
scoop install zulu8-jdk
```

### Troubleshooting

#### WinGet: 安装程序哈希不匹配

在管理员终端中运行：

```powershell
# 启用 InstallerHashOverride
winget settings --enable InstallerHashOverride
# 再次尝试安装
winget install <package> --ignore-security-hash
```

#### Scoop: Couldn't find manifest for ...

在现有的软件源中无法找到指定的软件

```powershell
scoop bucket list      # 查看已添加的 bucket
scoop buckwt add main  # 如果没有 main 的话，将 main 添加到 bucket
```