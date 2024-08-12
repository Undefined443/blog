[NeoVim 官网](https://neovim.io/)

## 安装

macOS

```sh
brew install neovim
```

Windows

- 使用 `winget`：

  ```powershell
  winget install Neovim.Neovim
  ```

- 也可以使用 `scoop`：

  ```powershell
  scoop install neovim
  ```

Ubuntu

- 通过包管理器：

  ```sh
  sudo apt install neovim
  ```

- 或者手动安装二进制包：

  ```sh
  curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux64.tar.gz
  sudo rm -rf /opt/nvim
  sudo tar -C /opt -xzf nvim-linux64.tar.gz
  ```

  将下面这行加入 `~/.bashrc` 或 `~/.zshrc`：

  ```sh
  export PATH="$PATH:/opt/nvim-linux64/bin"
  ```

参考：[neovim/INSTALL.md | GitHub](https://github.com/neovim/neovim/blob/master/INSTALL.md)

## 使用

```sh
nvim <your_file>
```

## [LazyVim](https://lazyvim.github.io/)

刚安装的 NeoVim 看起来和传统 Vim 并无两样。我们可以安装 LazyVim 插件来美化我们的 NeoVim。

### 安装

> 在安裝前确保你的系统已经安装了 GCC 或 Clang 等 C 编译器。

Linux / macOS：

```sh
git clone https://github.com/LazyVim/starter ~/.config/nvim
```

Windows：

```powershell
git clone https://github.com/LazyVim/starter $env:LOCALAPPDATA\nvim
```

### 配置

在 `~/.config/nvim/lua/config/lazy.lua` 文件下找到 `checker = { enabled = true }` 并改为 `checker = { enabled = true, notify = false }`，以关闭每次打开 NeoVim 时的插件更新弹窗。

```lua
  ...
  install = { colorscheme = { "tokyonight", "habamax" } },
  checker = {
    enabled = true, -- check for plugin updates periodically
    notify = false, -- notify on update
  }, -- automatically check for plugin updates
  performance = {
  ...
```

> 参考：[How do I stop "Plugin Updates" from appearing? | GitHub](https://github.com/LazyVim/LazyVim/discussions/3298)

在 `~/.config/nvim/lua/config/options.lua` 文件下可以进行一些个性化设置：

```lua
vim.opt.expandtab = true -- 使用空格代替 Tab
vim.opt.tabstop = 4      -- Tab键的宽度
vim.opt.shiftwidth = 4   -- 缩进时的宽度
vim.opt.softtabstop = 4  -- 按退格键时可以删除的空格数量
vim.g.lazyvim_plugin_notifications = false -- 禁用插件通知
```

> 将 `<your_file>` 替换为你要打开的文件名