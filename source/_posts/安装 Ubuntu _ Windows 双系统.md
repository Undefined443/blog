最近给自己的老笔记本换了一块大硬盘，顺便装了 Windows 和 Ubuntu 两个操作系统。记录一下安装过程。

> 💡 提示：Ubuntu 安装程序可以检测到磁盘已有的 Windows 安装。所以如果先安装 Windows，再安装 Ubuntu，可以免去稍后修改 GRUB 配置的流程。

## 安装 Ubuntu

1. 下载 Ubuntu Desktop 镜像文件。

   - 你可以在[官网](https://ubuntu.com/download/desktop)中使用标准下载；
   - 或者在[镜像源列表](https://launchpad.net/ubuntu/+cdmirrors)中就近下载，比如[清华源](https://mirrors.tuna.tsinghua.edu.cn/ubuntu-releases/)是很不错的选择；
   - 或者使用 [BitTorrent](https://ubuntu.com/download/alternative-downloads#bit-torrent) 下载。

2. 使用镜像文件制作启动 U 盘。

   制作启动盘的工具有很多。Ubuntu 官方推荐 [balenaEtcher](https://etcher.balena.io/)，因为它在 Windows、macOS 和 Linux 系统上均能运行。

3. 使用启动盘引导机器启动。

   - 将启动盘插入电脑，然后在开机时按住 `F12` 进入 BIOS 设置页面。

   - `F12` 是最常用的设置按键。如果不起作用，可以试试 `Esc`、`F2` 和 `F10`，或者根据你的电脑型号或主板型号查询对应的 BIOS 设置键。

4. 安装 Ubuntu。

   - 在安装时记得选择手动安装（Manual Installation），以便能够自己对硬盘分区并安装系统。

   - 安装 Ubuntu 时与 Windows 不同的一点是，对硬盘分区后要设置硬盘的挂载点。一般来说设置为根目录 `/` 即可。

   - 关于其他安装选项的介绍，详见 [Type of installation | Ubuntu Tutorials](https://ubuntu.com/tutorials/install-ubuntu-desktop#6-type-of-installation)。

## 安装 Windows

1. 制作启动 U 盘。

   - Microsoft 官方提供了媒介制作工具，可以自动下载镜像并制作启动盘。可以在 [Download Windows 11](https://www.microsoft.com/software-download/windows11) 页面的第二个选项“Create Windows 11 Installation Media”处下载。

   - 当然，你也可以使用安装 Ubuntu 时的方法，下载 ISO 镜像文件，然后再写入到启动盘。

2. 安装 Windows。过程不再赘述。

## 检查 BIOS 启动项

重启机器并打开 BIOS 设置。找到引导选项设置，在这里你会看到 BIOS 找到的引导选项以及它们的启动顺序。

在这里你应该看到两个引导选项：`Windows Boot Manager` 和 `Ubuntu`。

- `Windows Boot Manager` 是 Windows 的引导加载程序，你可以在 Windows 启动后对其进行配置。

- `Ubuntu` （实际是 GNU GRUB2），是 Linux 的引导加载程序，你可以在启动时或启动后对其进行配置。

如果你在这里缺少了 `Ubuntu`，则需要手动添加其引导文件。

点击 `Add Boot Option`（不同 BIOS 显示名称可能不一样）进入引导文件选择页，在这里你可以浏览磁盘上的所有 FAT32 分区的内容（是的，所谓 EFI 分区不过就是一个隐藏的 FAT32 分区罢了）。Ubuntu 的引导文件路径为 `/EFI/ubuntu/grubx64.efi`，但是如果你的 BIOS 启用了 Secure Boot 选项的话，则必须设置引导文件为 `/EFI/ubuntu/shimx64.efi`，通过它再启动真正的引导文件。

选中引导文件后，为该引导选项设置一个喜欢的名字，然后保存退出。

## 配置 GRUB

在启动电脑的时候注意观察一下 GRUB 启动选项中有没有 Windows Boot Manager。如果有的话则已经配置好了双系统。如果没有的话，我们需要更新 GRUB 配置。

启动 Ubuntu，编辑 GRUB 默认配置文件：

```sh
sudo vim /etc/default/grub
```

确保 `GRUB_TIMEOUT` 和 `GRUB_DISABLE_OS_PROBER` 选项如下设置：

```sh
GRUB_TIMEOUT=5                # 设置启动菜单显示时间
GRUB_DISABLE_OS_PROBER=false  # 启用操作系统检测工具
```

然后重新生成 GRUB 配置：

```sh
sudo update-grub
```

接下来重启电脑，你应该能在启动选项中看到 Windows Boot Manager。如果你希望启动 Windows 的话，选中这个选项就可以了。

```
                                         GNU GRUB  version 2.12
 Ubuntu
 Advanced options for Ubuntu
 Memory test (memtest86+x64.efi)
 Memory test (memtest86+x64.efi, serial console)
*Windows Boot Manager (on /dev/sda1)
 UEFI Firmware Settings
```

参见：

- [Install Ubuntu desktop | Ubuntu Tutorials](https://ubuntu.com/tutorials/install-ubuntu-desktop#1-overview)
