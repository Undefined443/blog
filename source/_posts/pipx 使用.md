`pipx` 用于在孤立环境中安装和运行 Python 应用程序。类似 Node.js 中的 `npx`。

## 安装

macOS:

```sh
brew install pipx
pipx ensurepath
```

Ubuntu:

```sh
sudo apt install pipx
pipx ensurepath
```

安装命令补全：

```sh
pipx completions # 遵循该命令的指引
```

## 使用

```sh
pipx install <package>   # 安装 Python 包
pipx uninstall <package> # 卸载 Python 包
pipx upgrade-all         # 升级所有已安装的包
pipx list                # 列出已安装的 Python 包
pipx reinstall-all       # 重新安装已安装的所有包
```

安装好需要的包后，直接输入包名即可使用包。例如：

```sh
pipx install cowsay      # 安装包
cowsay -t 'Hello, pipx!' # 使用包
```

## 搜索包

```sh
pipx install pypisearch
pypisearch <package>
```

参考：[在 Linux 中安装和使用 pipx | Linux 中国](https://linux.cn/article-15915-1.html)

## Troubleshooting

### 提示 home 路径中有空格

```
⚠️ Found a space in the home path. We heavily discourage this, due to multiple
    incompatibilities. Please check our docs for more information on this, as well as
    some pointers on how to migrate to a different home path.
1.6.0
```

解决方法：[Found a space in the home path - why the warning and how to fix](https://github.com/pypa/pipx/discussions/1330)

参见：[pipx 官网](https://pipx.pypa.io)