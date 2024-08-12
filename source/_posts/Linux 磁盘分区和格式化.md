## 分区

常用命令行工具：

- `fdisk`：适用于 MBR 分区表
- `gdisk`：适用于 GPT 分区表
- `parted`：适用于 MBR 和 GPT 分区表，功能更强大。它还有一个 GUI 版本，名为 `gparted`，使用起来更加方便。

> MBR 分区表已经被淘汰了，现在基本都使用 GPT 分区表。这篇文章将使用 `gdisk` 编辑 GPT 分区表。如果你的 Linux 有图形界面，建议使用 `gparted` 工具编辑分区，更加方便快捷。

安装必要的工具：

```sh
sudo apt install gdisk util-linux
```

列出所有物理磁盘及其分区表：

```sh
sudo parted -l
```

在上一步中选择你要进行分区操作的磁盘，例如 `/dev/sda`，然后进入交互式分区编辑界面：

```sh
sudo gdisk /dev/sda
```

常用命令：

- `p`：打印当前分区表
- `n`：新建分区
- `d`：删除分区
- `w`：保存并退出
- `q`：不保存直接退出

新建一个分区：

```sh
(gdisk) Command (? for help): n  # 新建分区
(gdisk) Partition number (5-128, default 5):  # 分区号，默认为第一个可用的分区号
(gdisk) First sector (3794108416-3907029134, default = 3794108416) or {+-}size{KMGTP}:  # 起始扇区，默认为磁盘空闲空间的起始位置
(gdisk) Last sector (3794108416-3907029134, default = 3907028991) or {+-}size{KMGTP}: +512G  # 结束扇区，指定为起始扇区的后 512 GiB 处
(gdisk) Current type is 8300 (Linux filesystem)
(gdisk) Hex code or GUID (L to show codes, Enter = 8300):  # 分区类型，默认为 Linux 文件系统类型
(gdisk) Changed type of partition to 'Linux filesystem'
```

这样，我们就创建了一个分区计划：新建分区 `/dev/sda5`，分区大小为 512 GiB，分区类型为 Linux 文件系统。

接下来应用这个分区计划，键入 `w` 保存并退出 `gdisk`。

> 💡 在新建分区时注意到需要指定分区类型。在 Windows 或 macOS 上分区过的小伙伴应该会发现以前并没有提示过要指定分区类型。实际上是因为软件将分区和格式化两个操作合并到一起了。在你为新分区指定文件系统时，分区软件自动匹配了其适合的分区类型。
>
> 那么分区类型的作用是什么？在 GPT 分区表中，每个分区都有一个类型 GUID，用来帮助操作系统和固件（如 UEFI）判断分区用途。比如说，类型为 EFI 的分区会被 UEFI 用来搜索引导程序。常用的分区类型有以下几种：
>
> - Linux Filesystem Data：十六进制代码 8300，常用文件系统 [ext4](https://zh.wikipedia.org/zh-cn/Ext4)
> - Microsoft Basic Data：十六进制代码 0700，常用文件系统 [NTFS](https://zh.wikipedia.org/zh-cn/NTFS), [FAT32](https://zh.wikipedia.org/wiki/%E6%AA%94%E6%A1%88%E9%85%8D%E7%BD%AE%E8%A1%A8), [exFAT](https://zh.wikipedia.org/zh-cn/ExFAT)

## 格式化

创建完分区后，需要先为分区建立文件系统，然后分区才能用来存储文件。下面是一些建立常见的文件系统的命令：

```sh
# ext4
sudo mkfs.ext4 /dev/sda5  # 格式化为 ext4
# exFAT
sudo apt-get install exfat-utils exfat-fuse  # 安装 exFAT 工具包
sudo mkfs.exfat /dev/sda5                    # 格式化为 exFAT

# NTFS
sudo apt install ntfs-3g  # 安装 NTFS 工具包
sudo mkfs.ntfs /dev/sda5  # 格式化为 NTFS
```

## 挂载

分区格式化好之后还需要挂载才能使用，就好像新买的 U 盘要插到电脑上才能用一样。

首先创建挂载点（例如 `/mnt/new_partition`）：

```sh
sudo mkdir -p /mnt/new_partition
```

接下来挂载分区：

```sh
sudo mount /dev/sda5 /mnt/new_partition
```

验证挂载：

```sh
df -h /mnt/new_partition
```

现在我们的新分区就已经被挂载且可以使用了。不过，现在的挂载只是临时挂载，当操作系统重启后这个挂载就失效了。为了能够在开机时自动挂载新分区，我们需要将新分区的 GUID 添加到自动挂载配置文件。

首先查找新分区的 GUID：

```sh
sudo blkid /dev/sda5
```

接下耒编辑 `/etc/fstab`，写入一条新的配置项：

```sh
UUID=your-guid /mnt/new_partition ext4 defaults 0 2
```

> 把 `your-guid` 替换为实际的 GUID。

测试新的 `fstab` 条目是否正确：

```sh
sudo mount -a
```

如果没有错误信息，说明配置正确。

## 分区的扩容和缩小

分区扩容和缩小的过程其实就是删除旧分区，然后新建一个大小不同，但分区编号和类型都相同的新分区的过程。

新建分区后，需要检查和调整文件系统以确保其完整性：

```sh
# ext4
sudo e2fsck -f /dev/sda5  # 检查文件系统
sudo resize2fs /dev/sda5  # 调整文件系统大小
```

详情可以参见 [Linux 扩展磁盘分区 | CSDN](https://www.cnblogs.com/Undefined443/p/18120277)。