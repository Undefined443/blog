### 更新 TeX Live

假设你的旧版 TeX Live 版本号为 2023，新版 TeX Live 版本号为 2024。你需要在下面的命令中相应地更改实际版本号。TeX Live 版本可以通过 `tlmgr --version` 命令查看。

```sh
# 移除链接文件
tlmgr path remove

# 重命名安装文件夹
mv /usr/local/texlive/2023 /usr/local/texlive/2024

# 下载更新脚本
wget https://mirror.ctan.org/systems/texlive/tlnet/update-tlmgr-latest.sh

# 运行更新脚本
PATH="/usr/local/texlive/2024/bin/x86_64-linux:$PATH" sh update-tlmgr-latest.sh -- --upgrade

# 临时 PATH
export PATH="/usr/local/texlive/2024/bin/x86_64-linux:$PATH"
```

检查 tlmgr 版本：

```sh
$ tlmgr --version
tlmgr revision 70671 (2024-03-17 02:10:09 +0100)
tlmgr using installation: /usr/local/texlive/2024
TeX Live (https://tug.org/texlive) version 2024
```

看到下面写着版本为 2024，而我现在处于 2024 年，说明更新成功了。

### 安装宏包

```sh
# 设置镜像源（可选）
tlmgr option repository https://mirrors.ustc.edu.cn/CTAN/systems/texlive/tlnet

tlmgr update --self --all  # # 更新宏包
tlmgr install scheme-full  # 安装全部宏包

# 重新生成 lualatex/fontspec 缓存
luaotfload-tool -fu

# 生成链接文件
/usr/local/texlive/2024/bin/x86_64-linux/tlmgr path add
```

> 官方不建议生成链接文件，而是建议将 TeX Live 的 bin 目录添加到 PATH。不过如果你是在更新 ShareLaTeX 容器的话，则必须生成链接文件。

参考：[Upgrade from TeX Live 2023 to 2024 | TeX Users Group](https://www.tug.org/texlive/upgrade.html)