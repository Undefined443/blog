Overleaf 官方的 [sharelatex](https://hub.docker.com/r/sharelatex/sharelatex/tags) 镜像的 TeX Live 版本可能较旧，无法安装最新的宏包，并且往往只包含了少量的基础宏包。为了方便使用，我们可以自己构建一个使用最新 TeX Live 的镜像，并一次安装全部宏包。下面是自制镜像的过程。

> ⚠️: 自制镜像需要 40 分钟左右

## 构建镜像

我们可以使用官方的配置构建新的镜像，或者在官方已经构建好的镜像的基础上重新构建自己的镜像。

如果你和我一样，使用 ARM 架构的芯片，那么你最好使用官方配置构建一个新的镜像。因为官方的 ShareLaTeX 镜像只有 amd64 版本。

### 使用官方配置构建镜像（推荐）

Overleaf 官方提供了一个仓库 [overleaf](https://github.com/overleaf/overleaf/)，该仓库包含了构建 ShareLaTeX 镜像的全部配置文件。不过，该配置只会安装基础宏包套件 `scheme-basic`。我们需要修改配置文件以安装全部宏包套件 `scheme-full`。

1. 克隆 `overleaf` 仓库：

   ```sh
   # 克隆仓库
   git clone https://github.com/overleaf/overleaf.git
   # 进入 overleaf/server-ce 目录
   cd overleaf/server-ce
   ```

2. 修改安装方案为 `scheme-full`：

   ```sh
   # 修改配置文件以使用 scheme-full 套件
   sed -i 's/scheme-basic/scheme-full/g' Dockerfile-base
   # TeX Live 换源（可选）
   sed -i 's@TEXLIVE_MIRROR=.*@TEXLIVE_MIRROR=https://mirrors.ustc.edu.cn/CTAN/systems/texlive/tlnet@g' Dockerfile-base
   ```

3. 安装依赖：

   （可选）在 `server-ce` 目录下新建一个目录 `fonts`，并将你需要用到的字体拷贝进来，然后在 `Dockerfile` 合适的位置添加如下命令：

   ```dockerfile
   ADD server-ce/fonts/ /usr/local/share/fonts/
   ```

   在上一条命令的下面添加如下命令，以安装宏包依赖：

   ```dockerfile
   RUN ln -s /usr/local/texlive/2024/texmf-var/fonts/conf/texlive-fontconfig.conf /etc/fonts/conf.d/09-texlive.conf && \
       fc-cache -fv && \
       apt-get update && \
       apt-get install -y inkscape python3-pygments && \
       echo "shell_escape = t" >> /usr/local/texlive/2024/texmf.cnf
   ```

   > - 记得将路径中的 `2024` 改为你当前最新版使用的年份
   >
   > - 你也可以在 APT 安装命令中添加 Windows 字体包 `ttf-mscorefonts-installer`
   >
   > - 提示：在使用 APT 安装软件包时你可能想要换源

4. 构建镜像

   ```sh
   make
   ```

   构建之后你会获得一个自己版本的 `sharelatex/sharelatex:latest` 镜像。为了方便后面使用我们最好重新给它设置一个名字：

   ```
   docker tag sharelatex/sharelatex:latest xxx/sharelatex:5.0.3
   ```

   > 将 `xxx` 替换为你喜欢的名字。版本号可以随意设置，不过要和之后运行 Overleaf 时 `toolkit/config/version` 中的版本号一致。

### 在官方镜像的基础上构建

新建一个 Dockerfile：

```dockerfile
FROM sharelatex/sharelatex:5.0.3

# 拷贝字体文件。把你需要用到的字体放到 fonts 文件夹
ADD fonts/ /usr/local/share/fonts/

# 更新 TeX Live
RUN cd /tmp \
&& tlmgr path remove \
&& mv /usr/local/texlive/2023 /usr/local/texlive/2024 \
&& wget https://mirror.ctan.org/systems/texlive/tlnet/update-tlmgr-latest.sh \
&& export PATH="/usr/local/texlive/2024/bin/x86_64-linux:$PATH" \
&& sh update-tlmgr-latest.sh -- --upgrade \
&& tlmgr update --self --all \
&& tlmgr install scheme-full \
&& luaotfload-tool -fu \
&& tlmgr path add \
&& ln -s /usr/local/texlive/2024/texmf-var/fonts/conf/texlive-fontconfig.conf /etc/fonts/conf.d/09-texlive.conf \
&& fc-cache -fv

# 安装依赖
RUN apt-get update && \
    apt-get install -y inkscape python3-pygments && \
    echo "shell_escape = t" >> /usr/local/texlive/2024/texmf.cnf

EXPOSE 80

ENTRYPOINT ["/sbin/my_init"]
```

> - 记得将所有路径中的 `2024` 改为你当前最新版使用的年份
>
> - 提示：在使用 APT 安装软件包时你可能想要换源

如果需要设置 TeX Live 镜像源，可以在上面配置中添加：

```sh
&& tlmgr option repository https://mirrors.ustc.edu.cn/CTAN/systems/texlive/tlnet \
```

即：

```dockerfile
RUN cd /tmp \
...
&& tlmgr option repository https://mirrors.ustc.edu.cn/CTAN/systems/texlive/tlnet && \
# 添加到 tlmgr update 之上
&& tlmgr update --self --all \
...
```

### 编译

```sh
docker build . --tag xxx/sharelatex:5.0.3
```

> 将 `xxx` 替换为你喜欢的名字。版本号可以随意设置，不过要和之后运行 Overleaf 时 `toolkit/config/version` 中的版本号一致。

## 启动 ShareLaTeX

1. 克隆 `toolkit` 仓库并初始化：

   ```sh
   git clone https://github.com/overleaf/toolkit.git
   cd toolkit
   bin/init  # 初始化
   ```

2. 检查 ShareLaTeX 版本，打开 `config/version`，检查其中的版本号和你之前构建的镜像的版本号是否一致。

3. ShareLaTeX 的启动脚本检测环境变量 `OVERLEAF_IMAGE_NAME` 来确定要使用的 ShareLaTeX 镜像。因此启动 ShareLaTeX 时需要将 `OVERLEAF_IMAGE_NAME` 变量设为你编译的镜像，然后再运行 `start` 脚本：

   ```sh
   OVERLEAF_IMAGE_NAME=xxx/sharelatex bin/start
   ```

   或者你嫌麻烦，可以直接在启动脚本中加入环境变量 `OVERLEAF_IMAGE_NAME`。编辑 `bin/docker-compose`：

   ```sh
   #! /usr/bin/env bash
   # shellcheck source-path=..

   # 加入下面这行
   OVERLEAF_IMAGE_NAME=xxx/sharelatex

   set -euo pipefail
   ```

   然后重新运行 `bin/up`，之后再用 `bin/start` 启动。

> 参考：[实践自部署 Overleaf](https://blog.sparktour.me/posts/2021/04/02/self-host-overleaf/)
> 相关阅读：[本地部署 Overleaf 服务](https://www.cnblogs.com/Undefined443/p/18155463)