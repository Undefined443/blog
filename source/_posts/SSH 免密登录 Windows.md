## 在远程 Windows 上安装并启动 OpenSSH 服务器

在开始之前请确保你的 Windows 已经安装了 OpenSSH 服务器。若没有安装，请在 `设置 > 系统 > 可选功能 > 添加可选功能` 中安装 `OpenSSH 服务器`。

安装完成后，在终端中输入以下命令启动 OpenSSH 服务器：

```powershell
# 启动 sshd 服务
Start-Service sshd
# 可以设置 sshd 服务自动启动
Set-Service -StartupType Automatic sshd
```

## 向远程 Windows 上传公钥

### 方法一

Windows 上的 OpenSSH 默认将管理员用户组的公钥验证文件设置为 `C:\ProgramData\ssh\administors_authorized_keys`。因此如果你登录的用户属于管理员组，那么将使用 `C:\ProgramData\ssh\administors_authorized_keys` 文件中的公钥而不是 `~\.ssh\authorized_keys` 文件中的公钥来验证你的身份。

因此我们需要把公钥上传到 `C:\ProgramData\ssh\administors_authorized_keys` 这个文件中。如果你的本地主机是 Linux/macOS，运行下面这条命令：

```sh
scp ~/.ssh/id_rsa.pub USER@HOST:/ProgramData/ssh/administrators_authorized_keys
```

> 将 `USER` 和 `HOST` 分别改为你自己的用户名和主机名

如果你的本地主机是 Windows，运行下面这条命令：

```sh
# 获取公钥
$authorizedKey = Get-Content -Path $env:USERPROFILE\.ssh\id_rsa.pub

# 要在远程主机上运行的命令，将公钥内容拷贝到远程主机的 administrators_authorized_keys 文件
$remotePowershell = "powershell Add-Content -Force -Path $env:ProgramData\ssh\administrators_authorized_keys -Value '''$authorizedKey''';icacls.exe ""$env:ProgramData\ssh\administrators_authorized_keys"" /inheritance:r /grant ""Administrators:F"" /grant ""SYSTEM:F"""

# 连接远程主机并运行 $remotePowershell 中的命令
ssh username@domain1@contoso.com $remotePowershell
```

参考：[OpenSSH for Windows 中基于密钥的身份验证 | Microsoft Learn](https://learn.microsoft.com/zh-cn/windows-server/administration/openssh/openssh_keymanagement?source=docs)

这里没有使用 `ssh-copy-id`，因为 `ssh-copy-id` 内部会执行一些 *nix shell 命令，因此只能用于 *nix 机器。

### 方法二

编辑 `C:\ProgramData\ssh\sshd_config`，在文件最底部，注释掉这两行配置：

```config
#Match Group administrators
#       AuthorizedKeysFile __PROGRAMDATA__/ssh/administrators_authorized_keys
```

此时所有用户的公钥验证文件都设置成了 `~/.ssh/authorized_keys`，此时再将公钥上传到远程 Windows 的 `~/.ssh/authorized_keys` 文件中：

```sh
scp ~/.ssh/id_rsa.pub USER@HOST:.ssh/authorized_keys
```

另外，你需要确保 `~\.ssh\authorized_keys` 文件的访问控制权限包括 `Everyone` 的读权限。如果没有 `Everyone` 的权限，你可以使用下面的命令添加 `Everyone` 的访问权限：

```powershell
icacls $Env:USERPROFILE\.ssh\authorized_keys /inheritance:r /grant Everyone:F
```

或者你可以打开资源管理器，在 `authorized_keys` 文件上右键 `属性 > 安全 > 编辑 > 添加 > 高级 > 立即查找` 中选中 `Everyone` ，再一路确定。

参考：[多台 WIN 10 之间的 SSH 免密登录 | 知乎](https://zhuanlan.zhihu.com/p/111812831)

## 登录远程 Windows

```powershell
ssh USER@HOST
```

在使用 SSH 登录 Windows 时，容易遇到一个巨坑就是不知道远程 Windows 的用户名。由于远程 Windows 可能设置了 Microsoft 帐户的原因，远程 Windows 的用户名可能不是用户主目录的名字。你需要使用下面的命令来查看用户名：

```powershell
$Env:USERNAME
```

而密码则使用 Microsoft 帐户的密码。

## 更改 SSH 登录 Windows 的默认 Shell（可选）

通过 SSH 连接到 Windows 时默认登录的 Shell 是 CMD，可以通过下面的命令将默认 Shell 改为 PowerShell：

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
# 可以使用 Remove-ItemProperty 删除这个属性
```

这种方法登录的是 PowerShell 5。如果将 `Value` 改为 `PowerShell 7` 的话在连接时会出错误提示：

```
Access is denied.
Connection to localhost closed.
```

参考：

- [适用于 Windows 的 OpenSSH 服务器配置 | Microsoft Learn](https://learn.microsoft.com/zh-cn/windows-server/administration/openssh/openssh_server_configuration#configuring-the-default-shell-for-openssh-in-windows)
- [通过 SSH 进行 PowerShell 远程处理 - PowerShell | Microsoft Learn](https://learn.microsoft.com/zh-cn/powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core?view=powershell-7.3)