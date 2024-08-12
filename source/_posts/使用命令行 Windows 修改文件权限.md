修改文件访问权限的命令行工具是 `icacls`，其使用语法是这样的：

```powershell
icacls <file>  # 查看文件的访问权限
icacls <file> /grant <SID>:<permission>  # 为用户显式授予权限
icacls <file> /deny <SID>:<permission>  # 为用户显式拒绝权限
icacls <file> /remove <SID>  # 删除用户的权限
icacls <file> /setowner <SID>  # 设置文件所属者
```

其中，`<file>` 是你要修改权限的文件，`<SID>` 是你要添加的用户名或用户 ID，`<permission>` 是你要为用户赋予的权限。

可以赋予的权限有：

- `F` - 完全访问
- `M` - 修改访问权限
- `RX` - 读取和执行访问权限
- `R` - 只读访问
- `W` - 只写访问

关于命令的更多用法可以查阅 [icacls | Microsoft Learn](https://learn.microsoft.com/zh-cn/windows-server/administration/windows-commands/icacls)，或者使用命令 `icacls /?` 查看帮助。

### 向文件添加管理员组和系统组的完全访问权限

```powershell
icacls <file> /inheritance:r /grant "Administrators:F" /grant "SYSTEM:F"
```

- `/inheritance:r`: 禁用继承

> 将 `<file>` 替换为你的文件名

### 向文件添加其他用户（组）的完全访问权限

如果不是为 `Administrators`、`Administrator`、`SYSTEM`、`everyone` 这种系统自带用户添加权限，建议使用用户的 SID 而不是用户名来添加权限。不然你可能会遇到这种 bug，添加的用户名变成一串 SID：

![image](https://img2024.cnblogs.com/blog/2778973/202404/2778973-20240422173624037-1675871355.png)

首先查询该用户的 SID：

```powershell
$ Get-LocalUser | Select-Object Name, SID
Name               SID
----               ---
Administrator      S-1-5-21-479362186-2444553748-1039381088-500
DefaultAccount     S-1-5-21-479362186-2444553748-1039381088-503
Guest              S-1-5-21-479362186-2444553748-1039381088-501
WDAGUtilityAccount S-1-5-21-479362186-2444553748-1039381088-504
MyUser             S-1-5-21-479362186-2444553748-1039381088-1001
```

或者查找用户组的 SID：

```powershell
$ Get-LocalGroup | Select-Object Name, SID
Name                                SID
----                                ---
docker-users                        S-1-5-21-479362186-2444553748-1039381088-1010
__vmware__                          S-1-5-21-479362186-2444553748-1039381088-1013
Access Control Assistance Operators S-1-5-32-579
Administrators                      S-1-5-32-544
Backup Operators                    S-1-5-32-551
Cryptographic Operators             S-1-5-32-569
Device Owners                       S-1-5-32-583
Distributed COM Users               S-1-5-32-562
Event Log Readers                   S-1-5-32-573
Guests                              S-1-5-32-546
Hyper-V Administrators              S-1-5-32-578
IIS_IUSRS                           S-1-5-32-568
Network Configuration Operators     S-1-5-32-556
Performance Log Users               S-1-5-32-559
Performance Monitor Users           S-1-5-32-558
Power Users                         S-1-5-32-547
Remote Desktop Users                S-1-5-32-555
Remote Management Users             S-1-5-32-580
Replicator                          S-1-5-32-552
System Managed Accounts Group       S-1-5-32-581
Users                               S-1-5-32-545
```

找到你的用户（组）对应的 SID，拷贝下来，然后运行命令：

```powershell
icacls <file> /inheritance:r /grant "*S-1-5-21-479362186-2444553748-1039381088-1001:F"
```

> 注意 SID 前面要有一个 `*` 号。