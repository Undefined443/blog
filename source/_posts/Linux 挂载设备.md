## 手动挂载

挂载：

```sh
# 创建挂载目录
sudo mkdir -p /path/to/mount
# 挂载
sudo mount /dev/sdX1 /path/to/mount
# 确认挂载
df -h
```

卸载：

```sh
sudo umount /path/to/mount  # 注意不要拼写为 unmount
```

挂载的对象可以是磁盘分区、ISO 映像文件、或者另一个目录。

例：将磁盘分区挂载到用户主目录

```sh
# 查看块设备信息
lsblk
# 创建临时挂载点
sudo mkdir -p /mnt/temp
# 挂载到临时挂载点
sudo mount /dev/sdb1 /mnt/temp
# 向复制用户数据
sudo rsync -aXS /home/john/ /mnt/temp/
# 卸载临时挂载点
sudo umount /mnt/temp
# 挂载到用户主目录
sudo mount /dev/sdb1 /home/john
# 确认挂载
df -h
# 权限检查和修复
sudo chown -R john:john /home/john
```

## 自动挂载

在 Linux 系统中，自动挂载文件系统（如外部硬盘、网络驱动器等）通常通过配置`/etc/fstab`文件或使用自动挂载工具（如`autofs`）来完成。

使用`/etc/fstab`文件的方法比较简单，适用于固定的挂载需求。而`autofs`则更为灵活，适用于动态挂载需求，如网络文件系统等。

### 使用 `/etc/fstab` 文件

`/etc/fstab` 文件包含了系统启动时需要自动挂载的文件系统的信息。通过编辑这个文件，可以添加新的挂载点。

1. 编辑 `/etc/fstab` 文件，指定要挂载的设备、挂载点、文件系统类型和挂载选项。格式如下：

   ```
   <设备>  <挂载点>  <文件系统类型>  <挂载选项>  <转储>  <fsck顺序>
   ```

   例如，要将一个 ext4 格式的分区自动挂载到 `/mnt/mydisk`：

   ```
   /dev/sdX1  /mnt/mydisk  ext4  defaults  0  2
   ```

   这里：

   - `/dev/sdX1` 是你的设备名称。
   - `/mnt/mydisk` 是挂载点。
   - `ext4` 是文件系统类型。
   - `defaults` 是挂载选项，表示使用默认选项。
   - `0` 表示不需要转储。
   - `2` 表示文件系统检查顺序。

2. 测试挂载配置：

   ```sh
   # 创建挂载点
   sudo mkdir -p /mnt/mydisk
   # 测试挂载配置
   sudo mount -a
   ```

   如果 `mount` 命令没有错误信息，说明配置正确。

### 使用 `autofs` 工具

`autofs` 是一个自动挂载守护进程，它会在需要时自动挂载文件系统，并在不再使用时自动卸载。

1. 安装 `autofs`：

   ```sh
   sudo apt install autofs
   ```

2. 编辑主配置文件 `/etc/auto.master`，指定挂载点和关联的映射文件：

   1. 编辑主配置文件：

      ```sh
      sudo vim /etc/auto.master
      ```

   2. 指定挂载点 `/mnt` 和关联的配置文件 `/etc/auto.misc`：

      ```
      /mnt /etc/auto.misc
      ```

2. 创建映射文件 `/etc/auto.misc`，添加挂载配置：

   1. 创建映射文件：

      ```sh
      sudo vim /etc/auto.misc
      ```

   2. 添加挂载配置，将 `/dev/sdX1` 挂载到挂载点下的子目录 `mydisk`：

      ```
      mydisk -fstype=ext4 :/dev/sdX1
      ```

3. 重启 `autofs` 服务：

   ```sh
   sudo systemctl restart autofs
   ```

现在，当你访问 `/mnt/mydisk` 目录时，`autofs` 会自动挂载设备。