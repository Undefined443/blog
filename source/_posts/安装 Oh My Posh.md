## Oh My Posh

[Oh My Posh 官网](https://ohmyposh.dev/)

### 安装

```powershell
winget install JanDeDobbeleer.OhMyPosh -s winget
```

> Oh My Posh 更新很快，有时会被杀毒软件误报，可以考虑将可执行文件路径 `(Get-Command oh-my-posh).Source` 加入杀毒软件的白名单。

### 更新

```powershell
winget install JanDeDobbeleer.OhMyPosh -s winget
```

### 安装字体

```powershell
oh-my-posh font install  # 在管理员权限下运行
```

> 官方推荐 `MesloLGM NF` 字体

### 启用 Oh My Posh

```powershell
notepad $PROFILE  # 编辑 PowerShell 配置文件
```

向配置文件中添加如下内容：

```powershell
oh-my-posh init pwsh | Invoke-Expression
```

Oh My Posh 的主题文件夹位置保存在环境变量 `$Env:POSH_THEMES_PATH` 中。可以通过命令以下命令查看。

```powershell
oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH\jandedobbeleer.omp.json"
```

## 附：安装 PowerShell 7

```powershell
winget install --id Microsoft.Powershell --source winget
```