### 安装 fish

```sh
brew install zsh
```

### 将默认 shell 切换为 fish

由于 Homebrew 安装的 fish 不在标准 shell 列表 `/etc/shell` 里，因此要先将 fish 的路径添加到 `/etc/shell`：

```sh
# fish 的路径改成你自己实际的路径
echo "/opt/homebrew/bin/fish" | sudo tee -a /etc/shells
```

接下来，更改默认 shell：

```sh
chsh -s /opt/homebrew/bin/fish
```

检查是否更改成功：

```sh
echo $SHELL  # /opt/homebrew/bin/fish
```