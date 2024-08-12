### 使用命令行安装

#### 安装 OpenSSH 服务端和客户端

在管理员终端下运行以下命令：

```powershell
# 检查 OpenSSH 可用性
Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'

# 安装 OpenSSH 客户端
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# 安装 OpenSSH 服务端
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

#### 启动 OpenSSH 服务端

```powershell
# 启动 sshd 服务
Start-Service sshd

# 可选，设置自动启动（推荐）
Set-Service -Name sshd -StartupType 'Automatic'

# 确认已配置防火墙规则。它应该通过安装程序自动创建。运行以下命令进行验证
if (!(Get-NetFirewallRule -Name "OpenSSH-Server-In-TCP" -ErrorAction SilentlyContinue | Select-Object Name, Enabled)) {
    Write-Output "Firewall Rule 'OpenSSH-Server-In-TCP' does not exist, creating it..."
    New-NetFirewallRule -Name 'OpenSSH-Server-In-TCP' -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
} else {
    Write-Output "Firewall rule 'OpenSSH-Server-In-TCP' has been created and exists."
}
```

也可以使用如下命令启动 OpenSSH 服务端（在管理员模式下运行）：

```batch
net start sshd
```

[MS Docs: 安装 OpenSSH](https://docs.microsoft.com/zh-cn/windows-server/administration/openssh/openssh_install_firstuse#start-and-configure-openssh-server)

### 使用图形界面安装

1. 搜索 “可选功能” 。
2. 添加可选功能 > 搜索 “SSH” ，并安装所有出现的选项。
3. 在管理员身份下打开 PowerShell，并运行上文中“启动 OpenSSH 服务端”部分的命令。
4. 接下来便可以使用 `ssh USER@HOST` 命令来连接到远程计算机。

> 如果要登录的远程计算机是 Windows，则用户名使用命令 `$Env:USERNAME` 给出的用户名，密码使用 Microsoft 帐户的密码（如果远程用户使用 Microsoft 帐户登陆的话）。

[远程登录 VMware 虚拟机](https://cloud.tencent.com/developer/article/1679861)
