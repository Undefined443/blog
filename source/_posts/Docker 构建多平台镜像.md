构建多平台镜像的方法分为两种：一种是在不同平台的机器上分别构建并推送对应平台的镜像，然后通过 Docker Manifest 将两个镜像标签合并为一个。另一种是通过 Docker buildx 在一台机器上构建并推送两个平台的镜像。

### 使用 Manifest 工具

此方法需要你先在对应架构的机器上分别构建和推送对应架构的镜像标签（例如：`tag-amd64`，`tag-arm64`），然后使用 `docker manifest` 合并这些镜像。

使用 `docker manifest` 命令手动创建和维护多架构镜像列表：

```bash
# 创建和维护清单文件
docker manifest create username/repository:tag \
  --amend username/repository:tag-amd64 \
  --amend username/repository:tag-arm64

# 推送 multi-architecture 镜像
docker manifest push username/repository:tag
```

#### 检查镜像

创建并推送完成后，你可以到 Docker Hub 上检查镜像是否正确标记了多个架构。

```sh
docker manifest inspect username/repository:tag
```

这个命令将展示镜像的元数据，包括它支持的架构。

### 使用 Docker buildx

Docker `buildx` 是 Docker 官方提供的一个插件，支持构建多平台镜像。你可以使用以下步骤创建一个支持多架构的镜像：

1. **创建并启动builder实例：**

   首先，确保你的 Docker 版本是最新的，并且已经开启了实验性功能（experimental features）。可以通过修改 Docker 的配置文件或设置环境变量 `DOCKER_CLI_EXPERIMENTAL=enabled` 来启用。

   ```sh
   export DOCKER_CLI_EXPERIMENTAL=enabled
   ```

   如果你还没有创建过 `buildx` builder 实例，需要首先创建一个：

   ```bash
   docker buildx create --name mybuilder --use  # 创建构建器
   docker buildx inspect --bootstrap  # 检查并启动构建器
   ```

2. **构建和推送镜像：**

   `buildx` 支持直接构建并推送多架构的镜像。例如，你可以在 arm64 机器上运行类似以下命令：

   ```bash
   # 使用 buildx 构建并推送镜像到 Docker Hub（或其他容器仓库）
   docker buildx build --platform linux/amd64,linux/arm64 -t username/repository:tag --push .
   ```

   这个命令会同时为 amd64 和 arm64 平台构建镜像，并将它们推送到指定的仓库和标签。

   > 构建的多平台镜像必须使用 `--push` 选项推送

#### 验证镜像架构

构建并推送后，你可以使用以下命令查看镜像支持的架构：

```bash
docker buildx imagetools inspect username/repository:tag
```

这将展示镜像标签下支持的所有架构。
