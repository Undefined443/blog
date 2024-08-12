## 安装桌面环境

如果你的 Ubuntu 还没有安装桌面环境，可以选择以下之一安装：

**GNOME**

GNOME 是 Ubuntu Desktop 原生桌面环境。

```sh
# 安装基本的 GNOME 桌面环境
sudo apt install -y gnome-session
# 或者安装全套的 GNOME 应用程序
sudo apt install -y ubuntu-desktop
```

远程连 GNOME 的速度可以说是超级慢。

![image](https://img2024.cnblogs.com/blog/2778973/202405/2778973-20240512133944836-1013730271.gif)

**KDE**

```sh
# 安装基本的 Plasma 桌面环境
sudo apt install -y kde-plasma-desktop
# 或者安装全套的 KDE 应用程序
sudo apt install -y kubuntu-desktop
```

远程连 KDE 的速度也很慢

![image](https://img2024.cnblogs.com/blog/2778973/202405/2778973-20240513130729106-2043129964.gif)

**Xfce**

Xfce 是一款轻量级的桌面环境。

```sh
# 安装基本的 Xfce 桌面环境
sudo apt install -y xfce4
# 或者安装全套的 Xfce 应用程序
sudo apt install -y xubuntu-desktop
```

相比之下 Xfce 的连接速度要快很多

![image](https://img2024.cnblogs.com/blog/2778973/202405/2778973-20240512151716179-1567268445.gif)

## 安装 xrdp

使用下面的命令安装 xrdp：

```sh
sudo apt install -y xrdp
```

一旦安装完成，xrdp 服务会自动启动。你可以用下面的命令来验证：

```sh
sudo systemctl status xrdp
```

如果你要连接的用户没有设置密码，你需要先设置密码：

```sh
sudo passwd $USER
```

接下来检查防火墙和安全组设置，确保开放了 3389 端口。

打开 RDP 软件，输入远程主机 IP 以及用户名和密码，连接到远程主机。

有时候连接一直黑屏，重启一下服务器就好了。

RDP 软件：

- [Microsoft Remote Desktop](https://apps.apple.com/us/app/microsoft-remote-desktop/id1295203466?mt=12)
- [Parallels Client](https://www.parallels.cn/products/ras/download/client/)

## 配置 xrdp

### 切换 X Window 会话桌面环境

xrdp 启动的桌面环境是通过 `~/.xsession` 文件配置的。 通过编辑 `~/.xsession` 文件可以控制我们使用 xrdp 连接远程桌面时使用的桌面环境。

编辑 `~/.session` 文件。

- 如果你需要使用 GNOME 桌面环境，将 `~/.xsession` 设置为：

   ```
   gnome-session
   ```

- 如果你需要使用 Xfce 桌面环境，将 `~/.xsession` 设置为：

   ```
   xfce4-session
   ```

编辑完成后，重启 xrdp 服务以应用更改：

```sh
sudo systemctl restart xrdp
```

### 查找可用桌面环境的方法

1. 检查可用的桌面环境会话文件：

   ```sh
   $ ls /usr/share/xsessions/
   plasma.desktop  ubuntu-xorg.desktop  ubuntu.desktop  xfce.desktop  xubuntu.desktop
   ```

2. 查看各个 `.desktop` 文件内容：

   ```sh
   $ cat ubuntu.desktop
   [Desktop Entry]
   Name=Ubuntu
   Comment=This session logs you into Ubuntu
   Exec=env GNOME_SHELL_SESSION_MODE=ubuntu /usr/bin/gnome-session --session=ubuntu
   TryExec=/usr/bin/gnome-shell
   Type=Application
   DesktopNames=ubuntu:GNOME
   X-GDM-SessionRegisters=true
   X-Ubuntu-Gettext-Domain=gnome-session-42
   ```

   其中 `Exec=xxx` 行就是启动桌面环境的命令。我们看它的启动命令就可以知道我们能启动什么桌面环境。在这里是 `/usr/bin/gnome-session`。因此我们可以在 `~/.xsession` 文件中填写 `gnome-session` 来启动 GNOME 桌面。

参考：

[Ubuntu Server 20.04 安装桌面(图形界面) 以及 远程桌面](https://blog.csdn.net/badboy_1990/article/details/121412618)

参见:

1. [Ubuntu 设置远程桌面（VNC）](https://i.cnblogs.com/posts/edit;postId=18127866)
2. [安装并配置 xrdp 以在 Ubuntu 上使用远程桌面 | Microsoft Learn](https://learn.microsoft.com/zh-cn/azure/virtual-machines/linux/use-remote-desktop?tabs=azure-cli)
3. [在Mac 使用远程桌面连接 Ubuntu 服务器 | 知乎](https://zhuanlan.zhihu.com/p/606588315)