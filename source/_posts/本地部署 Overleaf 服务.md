> ⚠️ 注意：本教程只适用于 macOS / Linux 操作系统。如果需要在 Windows 上部署 Overleaf，请先[安装 WSL](https://learn.microsoft.com/zh-cn/windows/wsl/install)，之后在 WSL 中部署 Overleaf 。

## 本地部署 Overleaf

### 启动

克隆 Overleaf 仓库：

```sh
git clone https://github.com/overleaf/toolkit.git
```

进入 `toolkit` 目录并生成配置文件：

```sh
bin/init
```

下载镜像并启动 Overleaf 容器：

```sh
bin/up
```

> 注意，Overleaf 脚本中使用的命令行工具均为 GNU 工具，如果你在 macOS 上启动 Overleaf 时遇到工具报错，有可能是你使用了 BSD 工具的原因。请使用 brew 安装相应工具的 GNU 版本，如 `coreutils`。

Overleaf 镜像下载完成并启动之后我们可以按下 `Ctrl` + `C` 停止容器。

之后启动 Overleaf 的时候，可以使用下面的命令让 Overleaf 在后台运行：

```sh
bin/start
```

参考：[Quick-Start Guide](https://github.com/overleaf/toolkit/blob/master/doc/quick-start-guide.md)

### 注册管理员帐户

Overleaf 容器启动之后，我们可以打开 http://localhost/launchpad 注册管理员帐户。之后我们可以用这个帐户登录 Overleaf 服务。

## 安装宏包

Overleaf 镜像自带的宏包很少，为了编译我们的 LaTeX 文档，往往需要安装其他宏包。

在**容器运行**的状态下进入容器 Shell：

```sh
bin/shell
```

更新并安装全部宏包：

```sh
# 使用中科大镜像源
tlmgr repository set https://mirrors.ustc.edu.cn/CTAN/systems/texlive/tlnet
# 更新已安装的全部宏包，包括 tlmgr 自身
tlmgr update --self --all
# 安装 scheme-full 套件（CTAN 上的全部宏包）
tlmgr install scheme-full
```

> ⚠️: 安装全部宏包的时间大约要 40 分钟左右

安装依赖包：

```sh
apt install ttf-mscorefonts-installer  # 安装 Windows 字体
apt install inkscape python3-pygments  # 安装 TikZ & Syntax highlighter 依赖

# 导入 TeX Live 字体
ln -s /usr/local/texlive/2024/texmf-var/fonts/conf/texlive-fontconfig.conf /etc/fonts/conf.d/09-texlive.conf
fc-cache -fsv  # 刷新字体缓存
```

你可能还要安装一些中文字体，将这些字体拷贝到 `fonts` 文件夹，然后运行下面的命令：

```sh
docker cp fonts sharelatex:/usr/local/share/fonts
```

参考：[搭建本地版 Latex 编辑器，为你的毕业论文排版「减减负」| 少数派](https://sspai.com/post/83982)

VS Code 有一个叫 Overleaf 的插件，可以让 VS Code 连接到你搭建的 Overleaf 服务。

## Troubleshooting

### 无法安装宏包

在安装宏包时你可能遇到如下错误：

```
tlmgr: Local TeX Live (2023) is older than remote repository (2024).
Cross release updates are only supported with
  update-tlmgr-latest(.sh/.exe) --update
See https://tug.org/texlive/upgrade.html for details.
   ```

这是镜像使用的 TeX Live 版本过低导致的。如果你当前使用的不是最新的 ShareLaTeX 镜像，可以使用下面的命令更新镜像：

```sh
bin/upgrade
```

如果你已经在使用最新的 ShareLaTeX 镜像，那说明官方还没有给最新的 TeX Live 打镜像。你可以设置 TeX Live 使用旧版镜像源，或者更新当前容器中的 TeX Live 版本，也可以自己打一个包含新版 TeX Live 的镜像。

- 设置旧版镜像源：

  ```sh
  tlmgr option repository https://mirrors.tuna.tsinghua.edu.cn/tex-historic-archive/systems/texlive/2021/tlnet-final/
  ```

- 如果你要更新当前容器中的 TeX Live 版本，参见：[Linux 更新 TeX Live](https://www.cnblogs.com/Undefined443/p/18156668)

- 如果你要自己打镜像，参见：[自制 ShareLaTeX 镜像](https://www.cnblogs.com/Undefined443/p/18156113)。

### 打开 Web 界面时遇到 502 错误

如果遇到 502 错误，可能是端口冲突导致的。进入 `config/overleaf.rc` 修改一下端口号：

```sh
OVERLEAF_PORT=8080
```

### 部署到服务器上之后无法访问

Overleaf 的默认监听端口是 `127.0.0.1:80`，也就是只监听来自本地的 HTTP 访问请求。如果要开放所有 IP 的访问请求，需要修改 `config/overleaf.rc` 设置：

```sh
OVERLEAF_LISTEN_IP=0.0.0.0
OVERLEAF_PORT=80
```

这样所有地址都可以访问服务器的 80 端口来获取 Overleaf 服务。

### 无法启动 ShareLaTeX 容器

如果你在运行 `bin/up` 的过程中遇到这样的错误提示：

```
Initiating Mongo replica set...
Rebranding from ShareLaTeX to Overleaf
  Starting with Overleaf CE and Server Pro version 5.0.0 the environment variables will use the Overleaf brand.
  Previous versions used the ShareLaTeX brand for environment variables.
  Your config/variables.env has 60 entries matching 'OVERLEAF_', expected prefix 'SHARELATEX_'.
  Please align your config/version with the naming scheme of variables in config/variables.env.
```

说明当前配置设置的 Overleaf 版本过低。我们需要使用大版本号为 5 以上的版本。

编辑 `config/version`，使用新的版本号替代旧的版本号。你可以在 Docker Hub 上查看最新的版本号：[sharelatex | DockerHub](https://hub.docker.com/r/sharelatex/sharelatex/tags)

撰写本文时，最新的版本号为：

```
5.0.3
```

### 无法启动 Mongo 容器

如果你在 WSL / macOS 上运行 `bin/up` 命令时遇到如下错误：

```
MongoNetworkError: connect ECONNREFUSED 127.0.0.1:27017
Waiting for Mongo...

Please configure ALL_TEX_LIVE_DOCKER_IMAGES in /home/xiao/toolkit/config/variables.env

You can read more about configuring Sandboxed Compiles in our documentation:
  https://github.com/overleaf/overleaf/wiki/Server-Pro:-sandboxed-compiles#changing-the-texlive-image
```

这可能是由于 Mongo 访问绑定挂载目录的 [bug](https://github.com/docker-library/docs/blob/master/mongo/content.md#where-to-store-data) 导致的。要解决这个问题，我们需要将 Mongo 和 Redis 的数据存储位置由绑定挂载目录改为命名卷。方法为：创建 `config/docker-compose.override.yml` 文件，填入以下内容：

```yaml
# 创建卷
volumes:
  mongo-data:
  redis-data:

# 挂载卷
services:
  mongo:
    volumes:
      - mongo-data:/data/db

  redis:
    volumes:
      - redis-data:/data
```

然后重新运行 `bin/up`。

参考：[Persistent Data | Overleaf Docs](https://github.com/overleaf/toolkit/blob/master/doc/persistent-data.md#volumes)