pip 是 Python 自带的包管理器，功能简单，对于大型项目来说缺少一些必要功能，不过对于个人小项目来说一般够用。

## 换源

### 临时换源

在安装模块时使用 `-i` 选项指定镜像源：

```sh
pip install -i https://pypi.mirrors.ustc.edu.cn/simple <package>  # 使用中科大源
```

其他可以使用的镜像源：

- 阿里云：`http://mirrors.aliyun.com/pypi/simple/`
- 中国科学技术大学：`https://pypi.mirrors.ustc.edu.cn/simple/`
- 清华大学：`https://pypi.tuna.tsinghua.edu.cn/simple/`
- 豆瓣：`http://pypi.douban.com/simple`

### 永久换源

```sh
pip config set global.index-url https://pypi.mirrors.ustc.edu.cn/simple
```

或者你可以手动修改 pip 配置文件：

- 对于 \*nix 系统，pip 的配置文件在 `~/.config/pip/pip.conf`
- 对于 Windows 系统，pip 的配置文件在 `%APPDATA%\pip\pip.ini`
- [PIP 配置文件参考文档](https://pip.pypa.io/en/stable/topics/configuration/)

### 部分镜像源参考文档

- [PyPI 镜像使用帮助 | 清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/)
- [PyPI 镜像源使用帮助 | 中国科学技术大学开源软件镜像](https://mirrors.ustc.edu.cn/help/pypi.html)

### Troubleshooting

使用非 HTTPS 加密源（如豆瓣源）安装模块时会报错，需要在命令最后加上 `--trusted-host pypi.douban.com`

或者在配置文件中手动加入 `trusted-host` 配置项：

```ini
[global]
index-url = http://pypi.douban.com/simple
[install]
trusted-host=pypi.douban.com
```

## 生成 requirements.txt

### 使用 pipreqs 包

`pipreqs` 包可以通过扫描项目目录帮助我们生成项目的依赖清单：

```sh
pip install pipreqs
pipreqs .  # 为当前项目生成依赖清单
```

### 使用 pip 自带功能

也可以使用 `pip freeze`，该命令将当前虚拟环境中安装的所有包及其版本号写入 `requirements.txt` 文件。（需要在虚拟环境下执行）

```sh
pip freeze > requirements.txt    # 生成当前环境的依赖清单
pip install -r requirements.txt  # 安装依赖
```

可以在 `requirements.txt` 顶部添加下面这行命令来指定镜像源：

```txt
-i https://pypi.tuna.tsinghua.edu.cn/simple
```

## Troubleshooting

### [Remove the pip search command](https://github.com/pypa/pip/issues/5216)

由于自 2020 年 11 月 14 日以来，PyPI XMLRPC API 持续收到过量的搜索调用，因此 `pip search` 命令将在不久的将来（撰稿日期 2022.9.26）弃用。目前要想使用 `pip search` 功能，可以在 [PyPI 官网](https://pypi.org) 进行搜索。