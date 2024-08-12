我们很多人使用 Mac 的原因之一是 macOS 是最像 Linux 的操作系统（bushi），而 macOS 精美的图形界面又让我们欲罢不能。那么能不能将 macOS 的图形界面搬到 Linux 上呢？对于一向以可定制化程度高而闻名的 Linux 来说这当然不是事。下面就以 Ubuntu 为例介绍更换 macOS Big Sur 桌面的方法。

![image](https://s2.loli.net/2024/07/07/OtPrH54ejio8WU2.png)

## 安装 Tweaks 和 Extensions 应用程序

为了修改 GNOME 桌面主题，我们需要用到 GNOME Tweaks 和 GNOME Shell Extensions 这两个应用程序。它们使我们能够定制和增强 GNOME 桌面体验。

GNOME Tweaks 为我们提供了那些在 GNOME 的标准系统设置中不可用的设置，而 GNOME Shell Extensions 则允许我们通过 GNOME 的官方网站 [extensions.gnome.org](https://extensions.gnome.org/) 安装用于增强并扩展 GNOME 桌面功能的插件。

可以通过下面这条命令来安装 GNOME Tweaks 和 GNOME Shell Extensions：

```sh
sudo apt install -y gnome-tweaks gnome-shell-extensions
```

## 安装插件

接下来我们打开 GNOME 的官网 [extensions.gnome.org](https://extensions.gnome.org/)，在这里我们可以安装受 GNOME Shell Extensions 应用程序管理的插件。为了能够在浏览器中与 GNOME Shell Extensions 应用程序交互，网站要求我们安装 GNOME Shell Integration 插件。我们点击 `Click here to install browser extension` 来安装插件。

![image](https://s2.loli.net/2024/07/07/28xVOhFj3pGLKrw.png)

### 自定义主题工具：User Themes

安装好浏览器插件后，我们在 GNOME 官网上找到 [User Themes](https://extensions.gnome.org/extension/19/user-themes/) 插件并启用。这可以用来加载用户目录中的主题：

![image](https://s2.loli.net/2024/07/07/upqt9jc4nPD6aCm.png)

![image](https://s2.loli.net/2024/07/07/GjksqMpPtncVDUA.png)

#### Big Sur 主题：WhiteSur-gtk-theme

接下来我们下载 macOS Big Sur 主题库。该主题库位于 GitHub 库 [WhiteSur-gtk-theme](https://github.com/vinceliuice/WhiteSur-gtk-theme/)，我们可以通过 git 下载：

```sh
git clone https://github.com/vinceliuice/WhiteSur-gtk-theme.git --depth=1
```

下载完成后进入主题目录，运行安装脚本并设置主题：

```sh
cd WhiteSur-gtk-theme  # 进入主题目录
./install.sh -t all -N glassy -s 220  # 运行安装脚本
sudo ./tweaks.sh -g  # 添加主题
```

#### Big Sur 应用图标：Mkos-Big-Sur

接下来我们下载 Mkos-Big-Sur 图标库。该图标库位于 GitHub 库 [Mkos-Big-Sur](https://github.com/zayronxio/Mkos-Big-Sur/)，我们可以使用 wget 工具下载：

```sh
wget https://github.com/zayronxio/Mkos-Big-Sur/releases/download/0.3/Mkos-Big-Sur.tar.xz
```

解压下载好的压缩包，并将图标文件移动到 `~/.icons` 目录：

```sh
mkdir ~/.icons  # 创建 ~/.icons 目录
tar -xJvf Mkos-Big-Sur.tar.xz -C ~/.icons  # 将图标文件解压到 ~/.icons 目录
```

#### Big Sur 壁纸：WhiteSur-wallpapers

接下来我们下载 WhiteSur-wallpapers 壁纸库。该壁纸库位于 GitHub 库 [WhiteSur-wallpapers](https://github.com/vinceliuice/WhiteSur-wallpapers)，我们可以使用 git 工具下载：

```sh
git clone https://github.com/vinceliuice/WhiteSur-wallpapers.git --depth=1
```

#### Big Sur 字体

##### 界面字体：SF Pro

macOS 的界面字体是 SF Pro。我们可以从 GitHub 库 [San-Francisco-Pro-Fonts](https://github.com/sahibjotsaggu/San-Francisco-Pro-Fonts) 下载：

```sh
git clone https://github.com/sahibjotsaggu/San-Francisco-Pro-Fonts.git --depth=1  # 下载字体库
sudo mkdir /usr/local/share/fonts/SF-Pro  # 新建字体文件夹
sudo mv SF-Pro* /usr/local/share/fonts/SF-Pro  # 安装字体
sudo fc-cache -fv  # 刷新字体列表缓存
```

##### 文档字体

macOS 的文档字体是 Helvetica，我们可以从 [font.download](https://font.download/font/helvetica-255) 下载：

```sh
wget https://font.download/dl/font/helvetica-255.zip
sudo mkdir /usr/local/share/fonts/Helvetica
sudo unzip helvetica-255.zip -d /usr/local/share/fonts/Helvetica
sudo fc-cache -fv
```

##### 代码字体：MesloLG

macOS 的代码字体（等宽字体）是 Menlo。Nerd Font 项目有一个 Menlo 的升级版字体，名为 [MesloLG](https://github.com/ryanoasis/nerd-fonts/tree/master/patched-fonts/Meslo/)。我们可以使用 wget 下载：

```sh
wget https://github.com/ryanoasis/nerd-fonts/releases/download/v3.2.0/Meslo.tar.xz
tar -xJvf Meslo.tar.xz
sudo mkdir /usr/local/share/fonts/Meslo
sudo mv MesloLG* /usr/local/share/fonts/Meslo
sudo fc-cache -fv
```

#### 使用 GNOME Tweaks 和系统设置设置主题、图标和壁纸

打开 GNOME Tweaks。GNOME Tweaks 位于 Utilities 文件夹里面。

![image](https://s2.loli.net/2024/07/07/pTYfQcrZNsLh6xa.png)

![image](https://s2.loli.net/2024/07/07/95sSgR6uCVdBecX.png)

在 Appearance 标签页中设置 Applications、Icons 和 Shell 项，如下所示：

![image](https://s2.loli.net/2024/07/07/zxSnCBFatKH3Xbp.png)

在 Fonts 标签页中设置字体，如下所示：

![image](https://s2.loli.net/2024/07/07/8IvlSdqkHtFOU1a.png)

在 Window Titlebars 标签页中设置 Placement 项，如下所示：

![image](https://s2.loli.net/2024/07/07/QCHgVBNObhM5LeX.png)

接下来进入系统设置：

![image](https://img2024.cnblogs.com/blog/2778973/202404/2778973-20240414030145794-1281275803.png)

在 Background 标签页中，点击 `Add Picture...`，将之前下载的 WhiteSur-wallpapers 壁纸导入进去，并应用新导入的壁纸。我这里导入了一张 `4k/Monterey-light.jpg` 壁纸：

![image](https://img2024.cnblogs.com/blog/2778973/202404/2778973-20240414031326351-1158966496.png)

在 Appearance 标签页中设置 Size、Position of New Icons、Panel mode、Icon size 和 Position on screen 项，如下所示：

![image](https://img2024.cnblogs.com/blog/2778973/202404/2778973-20240414030835683-1102778766.png)

### 毛玻璃效果：Blur my Shell

![image](https://img2024.cnblogs.com/blog/2778973/202404/2778973-20240414043519097-1446344618.png)

为了给启动台添加毛玻璃效果，我们需要安装 Blur my Shell 插件。打开 GNOME 的官网 [extensions.gnome.org](https://extensions.gnome.org/)，搜索并启用 [Blur my Shell](https://extensions.gnome.org/extension/3193/blur-my-shell/) 插件：

![image](https://img2024.cnblogs.com/blog/2778973/202404/2778973-20240414031919467-1156311150.png)

![image](https://img2024.cnblogs.com/blog/2778973/202404/2778973-20240414032732399-719354756.png)

### 神奇效果：Compiz alike magic lamp effect

![image](https://img2024.cnblogs.com/blog/2778973/202404/2778973-20240414043541075-575150039.png)

为了获得最小化窗口时的神奇效果，我们需要安装 Compiz alike magic lamp effect 插件。打开 GNOME 的官网 [extensions.gnome.org](https://extensions.gnome.org/)，搜索并启用 [Compiz alike magic lamp effect](https://extensions.gnome.org/extension/3740/compiz-alike-magic-lamp-effect/) 插件：

![image](https://img2024.cnblogs.com/blog/2778973/202404/2778973-20240414032822910-1466251982.png)

![image](https://img2024.cnblogs.com/blog/2778973/202404/2778973-20240414032905890-1290650618.png)

Ubuntu 的程序坞默认只能点击展开，不能点击隐藏窗口，可以使用如下命令开启点击隐藏：

```sh
gsettings set org.gnome.shell.extensions.dash-to-dock click-action 'minimize'
```

参考：[Ubuntu 22.04 桌面美化之 Mac Big Sur 风格 | 殇雪的博客](https://blog.nineya.com/archives/106.html)