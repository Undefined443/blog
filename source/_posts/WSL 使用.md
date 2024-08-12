WSL 是一个为在 Windows 10 和 Windows Server 2019 以上能够原生运行 Linux 二进制可执行文件（ELF 格式）的兼容层。可以把它当作一个只能用命令行交互的 Linux 虚拟机。

## 安装

参考：[安装 WSL | Microsoft Learn](https://learn.microsoft.com/zh-cn/windows/wsl/install)

在管理员模式下打开终端，输入：

```powershell
wsl --install
```

这将开始安装 Ubuntu。

安装好之后会提示你创建一个默认 UNIX 用户名和密码。用户名最好使用小写的单个单词：

```sh
Please create a default UNIX user account. The username does not need to match your Windows username.
For more information visit: https://aka.ms/wslusers
Enter new UNIX username:
New password:
Retype new password:
```

输入完成之后，我们的 Ubuntu 就初始化成功了：

```sh
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

Welcome to Ubuntu 22.04.3 LTS (GNU/Linux 5.15.133.1-microsoft-standard-WSL2 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage


This message is shown once a day. To disable it please create the
/home/user/.hushlogin file.
user@host:~$
```

如果要退出 WSL，输入 `exit`：

```sh
exit
```

如果下次要进入 WSL，输入 `wsl`，将进入设置的默认 Linux 发行版：

```powershell
wsl
```

## 常用 WSL 命令

```powershell
wsl --install   # 安装虚拟机（默认为 Ubuntu）
wsl --shutdown  # 停止所有 WSL 虚拟机
wsl --update    # 升级所有 WSL 虚拟机
wsl --list      # 查看虚拟机列表
wsl --unregister <vm-id>  # 卸载虚拟机
```

## 在 WSL 中使用 Docker

Windows 主机上的 Docker Desktop 可以和 WSL 虚拟机集成。也就是说，如果你的 Windows 主机已经安装了 Docker Desktop，那么就不用在 WSL 中再次安装 Docker（CE）了。我们只需在 Docker Desktop 设置中打开相关的虚拟机集成即可。

打开 Docker Desktop 设置，进入 Resources > WSL integration，然后打开你需要集成的虚拟机选项即可：

![image](https://img2024.cnblogs.com/blog/2778973/202404/2778973-20240415170755042-1133260300.png =800x)

参考：[WSL 上的 Docker 容器入门](https://learn.microsoft.com/zh-cn/windows/wsl/tutorials/wsl-containers)

## 设置镜像网络

如果你需要 WSL 虚拟机使用主机的网络环境，需要设置镜像网络。

在宿主机的用户主目录下创建文件 `.wslconfig`，填入以下配置：

```ini
[experimental]
autoMemoryReclaim=gradual  # gradual | dropcache | disabled
networkingMode=mirrored
dnsTunneling=true
firewall=true
autoProxy=true
```

然后重启 WSL 虚拟机：

```powershell
wsl --shutdown
```

参考：[WSL 2.0.0 Release Note](https://github.com/microsoft/WSL/releases/tag/2.0.0)

## WSL 连接 U 盘

安装 USBIPD-WIN 工具（需要 Windows 11 或更高）：

```powershell
winget install --interactive --exact dorssel.usbipd-win
```

以管理员模式打开 PowerShell，然后列出 USB 设备：

```powershell
usbipd list
```

找到你的 U 盘，然后共享并链接到 WSL：

```powershell
usbipd bind --busid <bus-id>         # 共享设备
usbipd attach --wsl --busid <busid>  # 链接设备
```

在 WSL 中验证设备已链接：

```sh
lsusb
```

使用完成后，可物理断开 USB 设备，或者从 PowerShell 运行命令：

```powershell
usbipd detach --busid <bus-id>
```

参考：[连接 USB 设备 | Microsoft Learn](https://learn.microsoft.com/zh-cn/windows/wsl/connect-usb)

## Troubleshooting

### Failed to translate xxx

安装了 Docker 的同学可能会发现使用 `wsl` 命令启动虚拟机时报错：

```powershell
$ wsl
<3>WSL (34) ERROR: CreateProcessParseCommon:708: Failed to translate C:\Users\user
<3>WSL (34) ERROR: CreateProcessParseCommon:754: getpwuid(0) failed 2
<3>WSL (34) ERROR: UtilTranslatePathList:2866: Failed to translate C:\Program Files\PowerShell\7
<3>WSL (34) ERROR: UtilTranslatePathList:2866: Failed to translate C:\Windows\system32
<3>WSL (34) ERROR: UtilTranslatePathList:2866: Failed to translate C:\Windows
...
```

这是因为 WSL 的默认 Linux 发行版被设置成了 `docker-desktop-data`。我们可以使用 `wsl -l` 命令检查默认 Linux 发行版：

```powershell
$ wsl -l
适用于 Linux 的 Windows 子系统分发:
docker-desktop-data (默认)
Ubuntu
docker-desktop
```

可以看到默认 Linux 发行版是 `docker-desktop-data`。我们可以使用 `wsl -s` 命令重新设置默认 Linux 发行版：

```powershell
wsl -s ubuntu  # 设置 Ubuntu 为默认发行版
```

### WSL2: WslRegisterDistribution failed with error: 0x80070002

我是在卸载了 VMware Workstation Pro 之后使用 WSL 安装 Ubuntu 时遇到了这样的问题。重新安装 VMware Workstation Pro，然后再卸载，然后再次使用 WSL 安装 Ubuntu 即可。

参考：[WSL2: WslRegisterDistribution failed with error: 0x80070002 | GitHub](https://github.com/microsoft/WSL/issues/6062#issuecomment-910183580)

### 虚拟机无法联网

编辑 WSL 配置文件 `/etc/wsl.conf`，加入下面两行配置：

```ini
[network]
generateResolvConf = false
```

然后重启虚拟机：

```powershell
wsl --shutdown
```

重新启动 WSL，并编辑 `/etc/resolv.conf` 文件，加入下面这行：

```ini
nameserver 223.5.5.5
```

参考：[WSL 无法访问网络的解决办法 | CSDN](https://blog.csdn.net/wbvalid/article/details/115540217)