## 使用

[Ubuntu 包搜索器](https://packages.ubuntu.com/)

|     apt 命令     | 功能                           |
| -------------- | :----------------------------- |
|   apt install    | 安装软件包                     |
|    apt remove    | 移除软件包                     |
|    apt purge     | 移除软件包及配置文件           |
|    apt update    | 刷新存储库索引                 |
|   apt upgrade    | 升级所有可升级的软件包         |
|  apt autoremove  | 自动删除不需要的包             |
| apt full-upgrade | 在升级软件包时自动处理依赖关系 |
|    apt search    | 搜索应用程序                   |
|     apt show     | 显示安装细节                   |

`whereis <package>`: 查找已安装的二进制包的位置

## 换源

> 关于 APT 源配置文件的介绍：[/etc/apt/sources.list 文件和 /etc/apt/sources.list.d 目录介绍 | 博客园](https://www.cnblogs.com/Undefined443/p/17976363)

旧版 Ubuntu（22.04 及以下）APT 的源配置文件位于 `/etc/apt/sources.list`，新版 Ubuntu（24.04 及之后）APT 的源配置文件迁移到了 `/etc/apt/sources.list.d/ubuntu.sources`。下面的命令默认使用旧版的位置，如果你是新版记得手动修改。

在换源之前，你可以先备份你的原始文件：

```sh
sudo cp /etc/apt/sources.list{,.bak}  # 文件备份到 sources.list.bak
```

### 使用配置文件换源

首先下载 APT 源配置文件 `sources.list`：

- 你可以直接使用现成的 `sources.list` 文件掉覆盖原来旧文件来换源。中科大镜像站给出了 Ubuntu 各个版本号的 `sources.list` 文件：[repository file generator](https://mirrors.ustc.edu.cn/repogen/)

  选择你的 Ubuntu 版本，复制配置文件，并粘贴到 `/etc/apt/sources.list`

- 你也可以使用自动检测工具 `netselect-apt` 来查找最快的源：

  ```sh
  sudo apt install netselect-apt  # 安装 netselect-apt 工具
  sudo netselect-apt              # 检测并下载最快源配置
  ```

  `netselect-apt` 工具会将 `sources.list` 文件下载到当前目录。用该文件替换 `/etc/apt/sources.list`：

  ```sh
  sudo mv sources.list /etc/apt/sources.list
  ```

最后不要忘了更新索引：

```sh
sudo apt update  # 更新索引
```

> 对于 ubuntu-ports 镜像，你只需将配置文件中的 `https://mirrors.xxxx.xxx/ubuntu/` 改为 `https://mirrors.xxxx.xxx/ubuntu-ports/` 即可。

### 使用命令换源

#### x86

> 该镜像仅适用于配置 x86 架构下的 Ubuntu系统，如果你的系统为 ARM，PowerPC 等其他架构，请使用 ubuntu-ports 源进行配置。

```sh
sudo sed -i 's@//.*archive.ubuntu.com@//mirrors.ustc.edu.cn@g' /etc/apt/sources.list  # 使用中科大源

# 因镜像站同步有延迟，可能会导致生产环境系统不能及时检查、安装上最新的安全更新，不建议替换 security 源：
sudo sed -i -r 's/security.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list  # 替换 security 源（可选）
```

然后更新 APT 索引：

```sh
sudo apt update  # 更新索引
```

可用的镜像站：

- [Ubuntu 源使用帮助 | 中国科学技术大学开源软件镜像](https://mirrors.ustc.edu.cn/help/ubuntu.html)
- [Ubuntu 软件仓库 | 清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)
- [Ubuntu 镜像 | 阿里巴巴开源镜像站](https://developer.aliyun.com/mirror/ubuntu)
- [Ubuntu镜像使用帮助 | 网易开源镜像站](https://mirrors.163.com/.help/ubuntu.html)

#### Ubuntu Ports

如果你使用 ARM，PowerPC 等架构的 Ubuntu 系统，请使用 ubuntu-ports 源进行配置：

```sh
sudo sed -i -r 's/ports.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list  # 使用中科大的 ubuntu-ports 源
```

然后更新索引：

```sh
sudo apt update  # 更新索引
```

可用的镜像站：

- [Ubuntu Ports 源使用帮助 | 中国科学技术大学开源软件镜像](https://mirrors.ustc.edu.cn/help/ubuntu-ports.html)
- [Ubuntu Ports 软件仓库 | 清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu-ports/)
- [Ubuntu Ports 镜像 | 阿里巴巴开源镜像站](https://developer.aliyun.com/mirror/ubuntu-ports)
- 网易没有提供 Ubuntu Ports 源的使用帮助

#### EOL 发行版换源

对于 EOL 发行版，需要使用 `old-releases.ubuntu.com` 源

> EOL: End Of Life，是那些过于古早的发行版，已经不再维护。

使用官方 old-releases 源（非镜像）：

```sh
sudo sed -i -r 's/([a-z]{2}\.)?archive.ubuntu.com/old-releases.ubuntu.com/g' /etc/apt/sources.list
sudo sed -i -r 's/security.ubuntu.com/old-releases.ubuntu.com/g' /etc/apt/sources.list
```

使用中科大镜像：

```sh
sudo sed -i 's@//.*archive.ubuntu.com/ubuntu@//mirrors.ustc.edu.cn/ubuntu-old-releases@g' /etc/apt/sources.list
sudo sed -i -r 's@security.ubuntu.com/ubuntu@mirrors.ustc.edu.cn/ubuntu-old-releases@g' /etc/apt/sources.list
```

然后更新 APT 索引：

```sh
sudo apt update  # 更新索引
```

可用的镜像站：

- [Ubuntu Old Releases 源使用帮助 | 中国科学技术大学开源软件镜像](https://mirrors.ustc.edu.cn/help/ubuntu-old-releases.html)
- [Ubuntu Old Releases 软件仓库 | 清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu-old-releases/)
- [oldubuntu-releases 镜像 | 阿里巴巴开源镜像站](https://developer.aliyun.com/mirror/oldubuntu-releases)
- [Ubuntu-releases 镜像使用帮助 | 网易开源镜像站](https://mirrors.163.com/.help/ubuntu-releases.html)

参见：

- [Ubuntu 各发行版版本代号](https://wiki.ubuntu.com/Releases)

## Troubleshooting

[apt-get 出现 Err 404 Not Found 的解决办法](https://www.cocobolo.top/linux/2019/04/26/apt-get%E5%87%BA%E7%8E%B0Err-404-Not-Found%E7%9A%84%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95.html)
