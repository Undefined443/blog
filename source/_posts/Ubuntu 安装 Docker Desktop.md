## 介绍

Docker 有两种版本：Docker Desktop 和 Docker Engine (也称作 Docker CE)。Docker Desktop 是带图形界面的版本，非常适合需要在桌面环境中进行容器开发和测试的开发者。Docker Engine 则只有命令行接口，适合在没有图形界面的服务器上进行容器开发和测试。

这篇文章将介绍在 Ubuntu Desktop 上安装 Docker Desktop 的方法。如果你需要在 Ubuntu Server 上安装 Docker 或者确定只需要 Docker Engine，请参阅 [Ubuntu 安装 Docker CE](https://www.cnblogs.com/Undefined443/p/18062149)。

## 安装

1. 添加 APT 仓库

    ```sh
    # 添加 Docker 官方的 GPG 公钥：
    sudo apt-get install -y ca-certificates curl  # 安装 curl 以及 curl 用到的 CA 证书
    sudo install -m 0755 -d /etc/apt/keyrings
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc  # 下载公钥文件
    sudo chmod a+r /etc/apt/keyrings/docker.asc  # 设置文件权限

    # 将 Docker 仓库添加到 APT 仓库中
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
      $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update  # 更新 APT 索引
    ```

2. 在 [Install Docker Desktop on Ubuntu](https://docs.docker.com/desktop/install/ubuntu/) 页面中点击 `DEB package` 按钮下载最新版 Docker Desktop `.deb` 文件

3. 进入 `.deb` 文件所在目录，运行：

    ```sh
    sudo apt-get install ./docker-desktop-x.x.x-amd64.deb  # 安装 Docker Desktop
    ```

    > 在安装时可能会收到如下警告：
    >
    > ```
    > N: Download is performed unsandboxed as root, as file '/home/user/Downloads/docker-desktop.deb' couldn't be accessed by user '_apt'. - pkgAcquire::Run (13: Permission denied)
    > ```
    >
    > 这是由于我们使用 `apt` 安装手动下载的包导致的，你可以忽略这个警告。

此时，你的 Docker Desktop 就安装完成了。你可以在程序菜单中找到 Docker Desktop。

## Troubleshooting

在安装完成之后，你可能会在打开 Docker Desktop 时遇到需要开启 [KVM](https://linux-kvm.org/page/Main_Page) 模块的报错。

你可以通过如下命令开启 KVM 模块：

```sh
modprobe kvm
```

然后根据你 CPU 的类型，运行下面其中一条命令：

```sh
modprobe kvm_intel  # Intel 处理器

modprobe kvm_amd    # AMD 处理器
```

如果上面的命令运行失败，你可以运行这条命令进行诊断：

```sh
sudo kvm-ok
```

> 如果你是在虚拟机里运行 Ubuntu，比如你正在使用 VMware Workstation 上的 Ubuntu 虚拟机安装 Docker，那么你很有可能是没有开启 `虚拟化 Intel VT-x/EPT 或 AMD-V/RVI` 功能。请你关闭该 Ubuntu 虚拟机（不是挂起），并在虚拟机设置中开启 `虚拟化 Intel VT-x/EPT 或 AMD-V/RVI`。
>
> 如果你在开启 `虚拟化 Intel VT-x/EPT 或 AMD-V/RVI` 时遇到错误，请参考 [VMware Workstation 开启虚拟化引擎
](https://www.cnblogs.com/Undefined443/p/18063678)

你可以通过下面的命令检查 `kvm` 模块和 `kvm_xxx` 模块是否安装成功：

```sh
$ lsmod | grep kvm
kvm_amd               167936  0
ccp                   126976  1 kvm_amd
kvm                  1089536  1 kvm_amd
irqbypass              16384  1 kvm
```

> 参考 [KVM virtualization support](https://docs.docker.com/desktop/install/linux-install/#kvm-virtualization-support)