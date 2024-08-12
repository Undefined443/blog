## 安装 Docker CE

> Docker CE 的安装方法参考自 [Install Docker Engine | Docker Docs](https://docs.docker.com/engine/install/)

Docker Engine (也称作 Docker CE) 是 Docker 官方的社区版包，它不包含在 Ubuntu 默认的存储库中。因此，你无法直接使用 `apt install docker-ce` 命令安装 `docker-ce`。你需要先添加 Docker 的官方 GPG 秘钥和存储库才能使用这个命令安装 `docker-ce`。

### 使用简易脚本安装

Docker 官方为我们编写了一个脚本，可以快速为我们完成上面的步骤。这个脚本的位置在 [get.docker.com](https://get.docker.com)

```sh
# 下载脚本
curl -fsSL https://get.docker.com -o install-docker.sh

# 执行脚本，这里通过 --mirror 选项指定使用阿里云镜像源加速安装
sudo sh install-docker.sh --mirror Aliyun
```

### 使用 APT 安装

如果你安装过 Docker 的非官方版本包的话，你需要先清理这些包：

```sh
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

添加 Docker 官方 GPG 公钥和 Docker 仓库：

```sh
# 添加 Docker 官方的 GPG 公钥
sudo apt-get install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings  # 创建 keyrings 目录，用来存放认证 APT 源的密钥环文件
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc  # 下载 Docker 的 GPG 公钥
sudo chmod a+r /etc/apt/keyrings/docker.asc

# 将 Docker 仓库添加到 APT 仓库列表
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

安装 Docker CE：

```sh
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

至此，Docker CE 就安装完成了。

参考：[Install Docker Engine on Ubuntu | Docker Docs](https://docs.docker.com/engine/install/ubuntu/#uninstall-docker-engine)

## 不使用 sudo 运行 docker

截至目前，你需要使用 `sudo` 来运行 `docker` 命令，就像这样：

```sh
sudo docker run hello-world
```

原因是 Docker 守护程序绑定到 Unix 套接字，而不是 TCP 端口。默认情况下，拥有 Unix 套接字的是 root 用户，其他用户只能使用 `sudo` 访问它。**Docker 守护程序始终以 root 用户身份运行。**

如果你不想每次在命令前加上 `sudo`，有以下两种方法可以实现你的需求：

### 将用户添加到 docker 组

如果不想在 `docker` 命令前加上 `sudo`，请创建一个名为 `docker` 的 Unix 组，并将用户添加到其中。当 Docker 守护程序启动时，它会创建一个 Unix 套接字，只有 `docker` 组的成员可以访问。

> ⚠️ 注意：该组为用户授予 root 级别权限。而这可能会影响系统安全
>
> 如果你希望让 Docker 在非 root 模式下运行，请参考下节 rootless mode

1. 创建 docker 用户组：
    ```sh
    sudo groupadd docker
    ```
    > 你的电脑上可能已经存在 `docker` 组，此时命令可能报错。你可以忽略这个错误。

2. 将你的用户添加到 docker 组中：
    ```sh
    sudo usermod -aG docker $USER
    ```

3. 注销并重新登录 Linux

4. 测试配置是否成功
    ```sh
    docker run hello-world
    ```
    命令的输出应该是 `Hello from Docker!` 以及一大行字符串

参考：[Linux post-installation steps for Docker Engine | Docker Docs](https://docs.docker.com/engine/install/linux-postinstall/)

### Rootless Mode（不推荐）

> ⚠️：Rootless Mode 由于缺少 root 权限，可能有些高级功能会受到限制。
>
> 目前已知的问题有：如果要运行的容器需要暴露 80 端口等特权端口，在没有 root 权限的情况下会失败。

####  安装

安装依赖：

```sh
sudo apt-get install -y uidmap
```

安装 rootless mode：

```sh
dockerd-rootless-setuptool.sh install
```

如果提示 `dockerd-rootless-setuptool.sh` 不存在，则可能需要手动安装 `docker-ce-rootless-extras` 包：

```sh
sudo apt-get install -y docker-ce-rootless-extras
```

> 参考：[Run the Docker daemon as a non-root user (Rootless mode) | Docker Docs](https://docs.docker.com/engine/security/rootless/)

#### 卸载

删除 Docker 守护程序的 systemd 服务：

```sh
dockerd-rootless-setuptool.sh uninstall
```

删除数据目录：

```sh
rootlesskit rm -rf ~/.local/share/docker
```

## 卸载 Docker CE

卸载 Docker CE：

```sh
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
```

删除残留镜像、容器、卷，以及自定义配置文件：

```sh
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```