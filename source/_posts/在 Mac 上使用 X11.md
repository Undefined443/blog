有时我们需要在服务器上运行一个 GUI 程序，然而我们是通过 SSH 连接到服务器的，看不到图形界面，怎么办呢？我们可以通过 X11 将 GUI 程序的界面转发到本地。

在 Mac 上使用 X11 需要安装 XQuartz，XQuartz 就是 macOS 下的 X11：

```sh
brew install --cask xquartz
```

如果你的服务器使用 Linux 系统，那么已经自带了 X11，不需要安装。

接下来使用 SSH 连接到服务器，并使用 `-X` 或 `-Y` 选项启用 X11 转发：

```sh
ssh -Y user@host  # ssh -Y 选项允许使用受信任的 X11 转发
```

> 将 `user@host` 替换为你自己要登录的远程主机的用户名和 IP 地址

这时，我们就可以运行一个 GUI 程序看看转发是否生效了。在服务器运行以下命令：

```sh
gedit &
```

如果一切正常，你会在本地主机上看到一个 gedit 窗口。

> 由于网络延迟的原因，你可能要等几秒至几十秒才能看到 X11 转发的窗口

### Troubleshooting

如果看到下面的报错，说明你可能没有设置 `DISPLAY` 环境变量：

```
Error: Can't open display: xxx
```

解决方法是先设置 `DISPLAY` 环境变量，再打开 SSH 连接：

```sh
export DISPLAY=:0  # 设置 X11 Server 为本地主机
```

参考：[macOS 安装并使用 XQuartz | 猫言猫语](https://www.wuweixin.com/2022/02/23/macos-install-and-using-xquartz/)