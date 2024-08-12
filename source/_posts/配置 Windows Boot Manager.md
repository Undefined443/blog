配置 Windows Boot Manager 通常需要使用 `bcdedit` 命令，这是一个命令行工具，用于管理 Boot Configuration Data (BCD) 存储。BCD 存储包含了启动配置数据，它决定了计算机在启动时加载哪些操作系统和启动选项。

## 查看当前的启动配置

首先，你可以查看当前的启动配置，以了解当前系统中有哪些启动项。在终端（管理员）中运行如下命令：

```batch
bcdedit /enum
```

## 添加新的启动项

假设你想添加一个新的启动项，例如一个新的 Windows 操作系统安装：

1. **创建新的启动项**：

   ```batch
   bcdedit /copy "{current}" /d "New Windows Installation"
   ```

   这会复制当前的启动项，并创建一个新的启动项，描述为 "New Windows Installation"。命令执行后，会返回一个新的 GUID（例如 `{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}`），你需要记住这个 GUID，因为接下来会用到。

2. **设置启动项的设备和路径**：

   假设你的新 Windows 安装在 `D:\Windows`，你可以设置设备和路径：

   ```batch
   bcdedit /set "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}" device partition=D:
   bcdedit /set "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}" osdevice partition=D:
   bcdedit /set "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}" path \Windows\system32\winload.exe
   ```

## 删除启动项

如果你不再需要一个启动项，可以将其删除：

```batch
bcdedit /delete "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
```

## 设置默认启动项

你可以设置一个默认的启动项，使其成为启动时自动选择的操作系统：

```batch
bcdedit /default "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
```

## 设置启动超时时间

你可以设置启动菜单显示的时间（以秒为单位），在这个时间内允许用户选择启动项：

```batch
bcdedit /timeout 30
```

## 恢复默认设置

如果你想恢复 BCD 存储的默认设置，可以使用以下命令：

```batch
bcdedit /import <path-to-backup-file>
```

你需要提供一个之前备份的 BCD 文件的路径。