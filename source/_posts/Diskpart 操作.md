`DiskPart` 是 Windows 操作系统中的一个命令行工具，用于管理磁盘分区。它可以创建、删除、格式化和调整分区大小，还可以设置活动分区等。以下是一些常用的 `DiskPart` 命令和操作步骤。

### 一、启动 DiskPart

1. **打开命令提示符**：

   - 按 `Win + R` 打开运行窗口，输入 `cmd` 并按回车。
   - 或者在开始菜单中搜索 `cmd`，右键点击以管理员身份运行。

2. **启动 DiskPart**：

   - 在命令提示符中输入 `diskpart` 并按回车。

### 二、常用命令

以下是一些常用的 `DiskPart` 命令及其说明：

```shell
list disk                     # 查看磁盘列表
select disk <disk_number>     # 选择磁盘

list partition                       # 查看分区列表
select partition <partition_number>  # 选择分区

create partition primary size=<size_in_MB>  # 创建分区
delete partition                            # 删除分区

format fs=<file_system> label=<volume_label> quick  # 格式化分区
active                        # 设置分区为活动分区
assign letter=<drive_letter>  # 分配驱动器号

list volume                   # 查看卷列表
select volume                 # 选择卷

clean                         # 清除磁盘
exit                          # 退出 Diskpart
```

- `<file_system>` 可以是 `NTFS`、`FAT32` 等。
- `<volume_label>` 是分区的卷标，例如 `MyData`。

### 三、示例操作

以下是一个完整的示例，演示如何使用 `DiskPart` 创建、格式化和分配驱动器号：

1. **启动 DiskPart**：

```shell
diskpart
```

2. **查看磁盘列表**：

```shell
DISKPART> list disk
```

3. **选择磁盘**（假设选择磁盘 0）：

```shell
DISKPART> select disk 0
```

4. **查看分区列表**：

```shell
DISKPART> list partition
```

5. **创建一个新的主分区**，大小为 20GB：

```shell
DISKPART> create partition primary size=20480
```

6. **选择新创建的分区**（假设分区编号为 1）：

```shell
DISKPART> select partition 1
```

7. **格式化分区**为 NTFS 文件系统，卷标为 MyData：

```shell
DISKPART> format fs=NTFS label=MyData quick
```

8. **分配驱动器号**（假设为 E 盘）：

```shell
DISKPART> assign letter=E
```

9. **退出 DiskPart**：

```shell
DISKPART> exit
```