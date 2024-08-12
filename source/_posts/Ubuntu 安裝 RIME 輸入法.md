[RIME](https://rime.im/) （Rime Input Method Engine，中州韻，中州韵）是一款很火的輸入法，虽然我目前还不知道它为什么火，不过先用用再说。

首先要吐槽一下 RIME 的说明文档，我感觉有点乱，第一次在 macOS 上尝试 RIME 的时候就被这说明文档劝退了，直接用了微信输入法。这次给自己的旧电脑安装 Ubuntu，决定再次尝试一下这款神秘的输入法。

## 简介

RIME 是一个输入法引擎，并不是一个完整的输入法程序。需要配合输入法前端才能使用。RIME 搭配不同的输入法前端，产生了不同的名字：

|中文名|英文名|平台|
|:--:|:--:|:--:|
| null | [ibus-rime](https://github.com/rime/ibus-rime) | Linux |
| 小企鹅 | [fcitx5-rime](https://github.com/fcitx/fcitx5-rime) | Linux |
|小狼毫|[Weasel](https://github.com/rime/weasel)| Windows|
|鼠须管|[Squirrel](https://github.com/rime/squirrel)|macOS|

Linux 上有两款输入法框架可以使用：[IBus](https://zh.wikipedia.org/zh-cn/IBus) 和 [Fcitx](https://zh.wikipedia.org/zh-cn/Fcitx)。其中 IBus 由 GNOME 团队开发，而 Fcitx 由国人开发，目前已经迭代到了 [Fcitx 5](https://fcitx-im.org/wiki/Fcitx_5)。根据我在一些论坛看到的讨论，大家对 Fcitx 的使用体验更加满意。需要注意的是，IBus 和 Fcitx 在 Linux 系统上不能共存。

RIME 中州韵有一个配置管理工具，名为东风破 [plum](https://github.com/rime/plum)。

> 不得不说这些名字都起的好奇怪

## 安装

### ibus-rime

`ibus-rime` 的安装很简单，只需要一条命令即可：

```sh
sudo apt install ibus-rime
```

安装好后，在 `设置` > `键盘` 中添加 Rime 输入法：

![image](https://img2024.cnblogs.com/blog/2778973/202408/2778973-20240803235059930-784967576.png)

![image](https://img2024.cnblogs.com/blog/2778973/202408/2778973-20240803235138804-1680159467.png)

### fcitx5-rime

`fcitx5-rime` 的安装则比较复杂：

```sh
# 安装 Fcitx5 框架和中文插件
sudo apt install fcitx5 fcitx5-chinese-addons
# 安装基于 Fcitx5 的 RIME 引擎
sudo apt install fcitx5-rime librime-plugin-lua
```

设置 `fcitx5` 开机自启，编辑 `~/.config/autostart/fcitx5.desktop`，输入下面的内容：

```ini
[Desktop Entry]
Type=Application
Exec=fcitx5
Hidden=false
NoDisplay=false
X-GNOME-Autostart-enabled=true
Name=Fcitx 5
Comment=Start Fcitx 5 input method framework
```

然后重启系统。

设置  Fcitx 5：

```sh
fcitx5-configtool
```

在右边 Available Input Method 处搜索 Rime，并将 Rime 移入左边 Current Input Method，并删除左边的其他输入方式，然后应用设置。

![image](https://img2024.cnblogs.com/blog/2778973/202408/2778973-20240801010110987-446185473.png)

## 配置

### 导入 oh-my-rime 配置

RIME 支持丰富的自定义选项。我们可以直接导入别人做好的配置来简化配置过程。这里我导入了 [oh-my-rime](https://github.com/Mintimate/oh-my-rime) 的配置。

首先安装东风破配置管理器：

```sh
curl -fsSL https://raw.githubusercontent.com/rime/plum/master/rime-install | bash
```

然后使用东风破导入配置：

```sh
cd plum
# IBus
./rime-install Mintimate/oh-my-rime:plum/full
# Fcitx 5
rime_frontend=fcitx5-rime bash rime-install Mintimate/oh-my-rime:plum/full
```

重新部署使配置生效：

```sh
# IBus
rm ~/.config/ibus/rime/default.yaml && ibus-daemon -drx
# Fcitx 5
rm ~/.local/share/fcitx5/rime/default.yaml && fcitx5-remote -r
```

### 添加自定义配置

oh-my-rime 的配置已经能很好地满足我们的需要了。不过我们还是可以微调一下以获得更好的使用体验。

首先进入配置目录：

```sh
# IBus
cd ~/.config/ibus/rime
# Fcitx 5
cd ~/.local/share/fcitx5/rime
```

RIME 主要有三种配置：

- `default.yaml`：RIME 内核配置。
- `squirrel.yaml`：Squirrel 前端设置（同理还有 `weasel.yaml` 和 `ibus_rime.yaml`）。
- `xxx.schema.yaml`：输入方案配置，`xxx` 为方案名。

当 RIME 更新时，这些配置会被覆盖，因此我们不应该编辑这些文件。RIME 允许我们编写 `.custom` 版本的补丁配置文件，这些文件将对原始文件的配置进行补充。也就是说，我们应该编写 `default.custom.yaml`、`squirrel.custom.yaml` 和 `xxx.custom.yaml`。

#### 修改 RIME 内核配置

在配置目录下编辑 `default.custom.yaml`：

```yaml
patch:
  # 要启用的输入方案
  schema_list:
    - schema: rime_mint             # 薄荷拼音
    - schema: double_pinyin_flypy   # 小鹤双拼
    - schema: rime_mint_flypy       # 薄荷拼音-小鹤混输方案
    - schema: terra_pinyin          # 地球拼音-薄荷定制
    - schema: wubi98_mint           # 五笔98-五笔小筑
    - schema: wubi86_jidian         # 五笔86-极点86
    - schema: t9                    # 仓九宫格-全拼输入
    - schema: double_pinyin_abc     # 智能ABC双拼
    - schema: double_pinyin_mspy    # 微软双拼
    - schema: double_pinyin_sogou   # 搜狗双拼
    - schema: double_pinyin_ziguang # 紫光双拼
    - schema: double_pinyin         # 自然码双拼
  menu:
    # 候选词个数
    page_size: 12
```

#### 修改输入方案配置

模糊拼音：

在配置目录下编辑 `rime_mint.custom.yaml`：

```yaml
patch:
  'speller/algebra':
    - erase/^xx$/ # 首选保留
    - derive/^([zcs])h/$1/ # zh, ch, sh => z, c, s
    - derive/^([zcs])([^h])/$1h$2/ # z, c, s => zh, ch, sh
    - derive/([aei])n$/$1ng/ # en => eng, in => ing
    - derive/([aei])ng$/$1n/ # eng => en, ing => in
    - derive/([iu])an$/$lan/ # ian => iang, uan => uang
    - derive/([iu])ang$/$lan/ # iang => ian, uang => uan
    - derive/([aeiou])ng$/$1gn/        # dagn => dang
    - derive/([dtngkhrzcs])o(u|ng)$/$1o/  # zho => zhong|zhou
    - derive/ong$/on/                  # zhonguo => zhong guo
    - abbrev/^([a-z]).+$/$1/ #简拼（首字母）
    - abbrev/^([zcs]h).+$/$1/ #简拼（zh, ch, sh）
```

参考：

- [Ubuntu 上安装使用 ibus-rime（超实用）| 博客园](https://www.cnblogs.com/keatonlao/p/12983158.html)
- [Ibus-rime vs fcitx-rime | V2EX](https://cn.v2ex.com/t/1010363)
- [fcitx 和 ibus 有啥不同？| Arch Linux 中文论坛](https://bbs.archlinuxcn.org/viewtopic.php?id=11248)
- [自由输入法 RIME 简明配置指南 | 少数派](https://sspai.com/post/84373)
- [oh-my-rime 输入法配置教程](https://www.mintimate.cc/zh/guide/installRime.html#linux%E5%AE%89%E8%A3%85rime)
- [Rime | Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/Rime)