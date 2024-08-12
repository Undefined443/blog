## 使用 `dd` 命令

`dd` 命令是 Unix 和 Unix-like 操作系统中用于低级别数据复制和转换的命令。它可以直接操作设备文件（如硬盘、光盘、USB 驱动器等），适用于备份、恢复、制作启动盘等任务。

1. **插入 U 盘**，并确定其设备名称：

   ```sh
   sudo fdisk -l
   ```

   假设你的U盘设备名称是`/dev/sdX`（`X`是一个字母，比如`/dev/sdb`）。

2. **确保U盘未挂载**。你可以使用以下命令来卸载 U 盘：

   ```sh
   sudo umount /dev/sdX
   ```

3. **使用 `dd` 命令写入 ISO 映像**。请注意，`/dev/sdX` 是 U 盘的设备名称，而不是分区名称（例如，不是`/dev/sdX1`）。

   ```sh
   sudo dd if=/path/to/your.iso of=/dev/sdX bs=4M status=progress && sync
   ```

   其中：

   - `if`：输入文件，即你要写入的 ISO 映像文件路径。
   - `of`：输出文件，即你的 U 盘设备名称。
   - `bs=4M`：设置块大小为 4MB。
   - `status=progress`：显示进度信息。
   - `&& sync`：确保写入完成。

## 使用`Startup Disk Creator` 工具

这是 Ubuntu 自带的工具，带有图形界面，操作比较简单。

1. **打开 Startup Disk Creator**。你可以通过应用菜单或在终端输入以下命令来打开：
   ```bash
   usb-creator-gtk
   ```

2. **选择 ISO 映像文件**。在窗口的顶部部分选择你的 ISO 文件。

3. **选择目标 U 盘**。在窗口的底部部分选择你的 U 盘。

4. **点击“Make Startup Disk”**。然后按照提示进行操作。

## 使用 `Etcher`

Etcher 是一个跨平台的图形化工具，操作简单且安全。

1. **下载 Etcher**。你可以从[Etcher官方网站](https://www.balena.io/etcher/)下载适用于Linux的版本。

2. **安装 Etcher**。下载完成后，解压并运行安装文件。

3. **运行 Etcher**。打开 Etcher 应用程序。

4. **选择 ISO 映像文件**。点击“Flash from file”按钮，然后选择你的 ISO 文件。

5. **选择目标 U 盘**。点击“Select target”按钮，然后选择你的 U 盘。

6. **开始写入**。点击“Flash!”按钮，然后等待写入完成。