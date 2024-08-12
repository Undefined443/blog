Poetry 是当下热门的 Python 包管理器。Poetry 注重为项目提供完整的生命周期管理，包括构建、打包、发布和依赖管理。其使用 `pyproject.toml` 文件来管理项目的依赖和构建配置。

相比 Pipenv，Poetry 更有前景一些。

## 安装

```sh
pipx install poetry
```

### 命令补全

```sh
mkdir $ZSH_CUSTOM/plugins/poetry
poetry completions zsh > $ZSH_CUSTOM/plugins/poetry/_poetry
```

然后，你必须将 `poetry` 添加到你的 `~/.zshrc` 中的 plugins 数组：

```
plugins(
    poetry
    ...
)
```

## 使用

### 创建新项目

```sh
poetry new <project-name> # 新建项目目录
```

### 初始化已经存在的项目

```sh
poetry init
```

### 安装依赖

```sh
poetry install
```

### 激活虚拟环境

```sh
poetry shell # 激活虚拟环境
exit         # 退出虚拟环境
```

### 移除虚拟环境

```sh
poetry env remove
```

参考：[Basic Usage | Poetry Docs](https://python-poetry.org/docs/basic-usage/)

参见：[Poetry 官网](https://python-poetry.org/docs)