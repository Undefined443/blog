chezmoi（发音 /ʃeɪ mwa/ (shay-moi)）：在多台不同的机器上安全地管理你的 dotfiles。

## 安装

macOS:

```sh
brew install chezmoi
```

Ubuntu:

```sh
snap install chezmoi --classic
```

## 在单台机器上使用

### 初始化

```sh
chezmoi init
```

这将在 `~/.local/share/chezmoi` 中创建一个新的 git 本地仓库，chezmoi 将在其中存储其源代码状态。默认情况下，chezmoi 只修改工作副本中的文件。

### 管理文件

```sh
chezmoi add ~/.bashrc
```

这将把 `~/.bashrc` 复制到 `~/.local/share/chezmoi/dot_bashrc`。

```sh
chezmoi forget ~/.bashrc
```

这将移除 `~/.local/share/chezmoi/dot_bashrc`。

### 进入本地仓库

```sh
chezmoi cd
git add .
git commit -m "Initial commit"
```

### 推送远程库

在 GitHub 上创建一个名为 `dotfiles` 的[新仓库](https://github.com/new)，然后推送你的仓库：

```sh
git remote add origin git@github.com:$GITHUB_USERNAME/dotfiles.git
git branch -M main
git push -u origin main
```

推送完成后，退出本地仓库：

```sh
exit
```

## 在多台机器上使用

### 初始化

用你的 `dotfiles` 仓库初始化 chezmoi：

```sh
chezmoi init https://github.com/$GITHUB_USERNAME/dotfiles.git
```

### 查看变更

查看 chezmoi 会对你的主目录做出哪些更改：

```sh
chezmoi diff
```

### 应用更改

```sh
chezmoi apply -v
```

参考：[chezmoi 官网](https://www.chezmoi.io/)