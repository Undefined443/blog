## 简介

**Conda 和 Anaconda、Miniconda 的关系**

- Conda 是一个包管理器及环境管理器。
- Anaconda 和 Miniconda 都是一种 Python 和 R 发行版，其包括了 Conda。
- Miniconda 是 Anaconda 的一个精简版。

**Conda 和 Python 的关系**

Conda 给我的感觉就像 Python 上的 NVM，可以管理多个 Python 版本。Python 的 pip 相当于 Node.js 的 npm。

## 安装

如果只需要 Conda 和 Pyhton，我们安装 Miniconda 就可以了：

macOS:

```sh
brew install miniconda
```

Ubuntu:

```sh
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh
```

```sh
~/miniconda3/bin/conda init zsh
```

参考：[Quick command line install | Anaconda](https://docs.anaconda.com/miniconda/#quick-command-line-install)

## 使用

### 环境管理

> 安装 Conda 时会自动创建一个 base 环境。为了避免包版本冲突，我们最好创建自己的环境并在其中安装包。

创建一个新的 Conda 环境：

```sh
conda create --name myenv
conda create --name myenv python=3.8 # 可以指定 Python 版本
```

激活 Conda 环境：

```sh
conda activate myenv
```

停用当前激活的 Conda 环境：

```sh
conda deactivate
```

列出所有 Conda 环境：

```sh
conda env list
```

删除一个 Conda 环境：

```sh
conda remove --name myenv --all
```

### 包管理

安装包到当前激活的环境中：

```sh
conda install <package>
conda install <package>=<version> # 指定包版本
```

从当前激活的环境中卸载包：

```sh
conda remove <package>
```

更新当前激活环境中的包：

```sh
conda update <package>
conda update conda # conda 自身也是一种包
```

列出当前激活环境中的所有包：

```sh
conda list
```

### 导出环境

查看当前激活环境的详细信息：

```sh
conda info
```

将当前环境导出为一个 YAML 文件：

```sh
conda env export > environment.yml
```

从一个 YAML 文件创建环境：

```sh
conda env create -f environment.yml
```

### 清理缓存

清理 Conda 缓存以释放空间：

```sh
conda clean --all
```

## 换源

```sh
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2/
conda config --set show_channel_urls yes
```

```sh
conda clean -i # 清除索引缓存，保证用的是镜像站提供的索引。
```

Conda 的配置文件为 `~/.condarc`。

- [Anaconda 清华镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/)
- [Anaconda 中科大镜像站](https://mirrors.ustc.edu.cn/help/anaconda.html)（已停用）