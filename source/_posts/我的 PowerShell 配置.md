安装 [Scoop](https://scoop.sh/)：

> Scoop 是 Windows 上的包管理器

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
```

安装 [Oh My Posh](https://ohmyposh.dev/)：

> Oh My Posh 是一款 Shell 美化插件

```powershell
winget install JanDeDobbeleer.OhMyPosh -s winget
```

PowerShell 配置文件位置：

`C:\%USERPROFILE%\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1`