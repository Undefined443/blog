man 命令显示的命令手册默认是没有颜色的。为了使 man 命令的输出更为生动，可以使用如下两种方法修改 man 命令的颜色配置。

### 方法一：设置环境变量

在你的 `.zshrc` / `.bashrc` 中添加以下行：

```sh
export LESS_TERMCAP_mb=$'\e[1;32m' \
       LESS_TERMCAP_md=$'\e[1;32m' \
       LESS_TERMCAP_me=$'\e[0m' \
       LESS_TERMCAP_se=$'\e[0m' \
       LESS_TERMCAP_so=$'\e[01;33m' \
       LESS_TERMCAP_ue=$'\e[0m' \
       LESS_TERMCAP_us=$'\e[1;4;31m'
```

环境变量解释：

- `LESS_TERMCAP_mb`：开始闪烁模式的转义序列。
- `LESS_TERMCAP_md`：开始加粗模式的转义序列。
- `LESS_TERMCAP_me`：结束模式（加粗、闪烁等）的转义序列。
- `LESS_TERMCAP_se`：结束突出文本（例如高亮语句）的转义序列。
- `LESS_TERMCAP_so`：开始突出文本（例如高亮语句）的转义序列。
- `LESS_TERMCAP_ue`：结束下划线模式的转义序列。
- `LESS_TERMCAP_us`：开始下划线模式的转义序列。

接下来在新打开的终端中使用 `man` 命令即可看到带有颜色的命令手册。

PS：为了避免环境变量污染，我们可以把环境变量通过 `env` 命令传递到 `man` 程序的运行环境中，再把这条命令设置成一个函数：

```sh
function man {
    env \
        LESS_TERMCAP_mb=$'\E[1;32m' \
        LESS_TERMCAP_md=$'\E[1;32m' \
        LESS_TERMCAP_me=$'\E[0m' \
        LESS_TERMCAP_se=$'\E[0m' \
        LESS_TERMCAP_so=$'\E[01;33m' \
        LESS_TERMCAP_ue=$'\E[0m' \
        LESS_TERMCAP_us=$'\E[1;4;31m' \
            man "$@"
}
```

### 方法二：使用 MOST 分页程序

安装 [MOST](https://www.jedsoft.org/most/)：

```sh
brew install most
```

在 `.zshrc` / `.bashrc` 中添加如下内容：

```sh
export PAGER="most"
```

接下来在新打开的终端中使用 `man` 命令即可看到带有颜色的命令手册。

---

参考：[How to View Colored Man Pages in Linux | TecMint](https://www.tecmint.com/view-colored-man-pages-in-linux/)

关于 LESS 环境变量的详细解释：[Colorized man pages: Understood and customized | Idle Time](https://boredzo.org/blog/archives/2016-08-15/colorized-man-pages-understood-and-customized)