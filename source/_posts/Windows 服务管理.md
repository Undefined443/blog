### 创建服务

```powershell
New-Service -Name NAME -BinaryPathName COMMAND -StartupType Automatic -Description DESCRIPTION
```

> 将 `NAME` 替换为服务的名称，将 `COMMAND` 替换为服务要运行的命令，将 `DESCIPTION` 替换为你对服务的描述。

### 启动/停止服务

```powershell
Start-Service -Name NAME  # 启动服务
Stop-Service -Name NAME  # 停止服务
Restart-Service -Name NAME  # 重启服务
Get-Service -Name NAME  # 查询服务状态
```

### 重新配置服务

```powershell
Set-Service -Name NAME -StartupType Manual
```

### 删除服务

```powershell
Remove-Service -Name NAME
```