## 基本配置

安裝 Zsh：

```sh
# Ubuntu/Debian
sudo apt install zsh

# macOS
brew install zsh
```

> macOS 默认使用 Zsh，可以不用重复安装。

安装配置框架 [Oh My Zsh](https://ohmyz.sh/):

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## 插件

- 安装插件 [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)：

  ```sh
  git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
  ```

  在 `~/.zshrc` 中激活插件：

  ```sh
  plugins=( [其他 plugin...] zsh-syntax-highlighting)  # zsh-syntax-highlighting 必须放在最后
  ```

- 安装插件 [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)：

  ```sh
  git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
  ```

  在 `~/.zshrc` 中激活插件：

  ```sh
  plugins=( [其他 plugin...] zsh-autosuggestions)
  ```

- 安装插件 `zsh-proxy`：

  ```sh
  git clone https://github.com/sukkaw/zsh-proxy.git ~/.oh-my-zsh/custom/plugins/zsh-proxy
  ```

  在 `~/.zshrc` 中激活插件：

  ```sh
  plugins=( [其他 plugin...] zsh-proxy)
  ```

- 安装 Zsh 主题 [Powerlevel10k](https://github.com/romkatv/powerlevel10k)

  1. 下载并安装字体 MesloLG Nerd Font：

     - [MesloLGMNerdFontMono-Regular.ttf](https://github.com/ryanoasis/nerd-fonts/raw/master/patched-fonts/Meslo/M/Regular/MesloLGMNerdFontMono-Regular.ttf)
     - [MesloLGMNerdFontMono-Bold.ttf](https://github.com/ryanoasis/nerd-fonts/raw/master/patched-fonts/Meslo/M/Bold/MesloLGMNerdFontMono-Bold.ttf)
     - [MesloLGMNerdFontMono-Italic.ttf](https://github.com/ryanoasis/nerd-fonts/raw/master/patched-fonts/Meslo/M/Italic/MesloLGMNerdFontMono-Italic.ttf)
     - [MesloLGMNerdFontMono-BoldItalic.ttf](https://github.com/ryanoasis/nerd-fonts/raw/master/patched-fonts/Meslo/M/Bold-Italic/MesloLGMNerdFontMono-BoldItalic.ttf)

     ```sh
     mv MesloLGMNerdFontMono-*.ttf ~/.local/share/fonts
     ```

  2. 打开终端设置，启用 Custom font 并设置为 `MesloLGM Nerd Font Mono`

     ![image](https://img2024.cnblogs.com/blog/2778973/202407/2778973-20240731222716200-1179435270.png)

  3. 克隆 Powerlevel10k 存储库：

     ```sh
     git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
     ```

  4. 在 `~/.zshrc` 中更改主题设置：

     ```sh
     ZSH_THEME="powerlevel10k/powerlevel10k"
     ```