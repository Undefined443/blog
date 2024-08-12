### 安装新版本

```sh
cd /tmp
# 下载安装压缩包
wget https://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz
# 解压压缩包
zcat < install-tl-unx.tar.gz | tar xf -
# 进入解压目录
cd install-tl-*
# 安装，使用中科大镜像源
perl ./install-tl --no-interaction --location https://mirrors.ustc.edu.cn/CTAN/systems/texlive/tlnet
# 将 bin 目录添加到环境变量 PATH
export PATH="/usr/local/texlive/2024/bin/x86_64-linux:$PATH"
```

默认情况下，所有内容都会被安装（7+GB）。需要等待较长时间。

### 卸载旧版本

如果你安装过旧版本的 TeX Live（假如是 TeX Live 2023），使用如下命令卸载 TeX Live 2023：

```sh
/usr/local/texlive/2023/bin/x86_64-linux/tlmgr remove --all --force
```

> ⚠️: 这将删除整个 TeX Live 2023 目录。

参考：[TeX Live - Quick install for Unix](https://www.tug.org/texlive/quickinstall.html)