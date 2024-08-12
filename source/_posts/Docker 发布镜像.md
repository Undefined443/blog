## 发布镜像

### 在 Docker Hub 发布镜像

1. 登陆到 Docker Hub

   ```sh
   docker login
   ```

2. 标记镜像并推送到 Docker Hub

   ```sh
   docker tag <image>:<tag> <username>/<image>:<tag>
   docker push <username>/<image>:<tag>
   ```

### 在 GitHub 发布镜像

1. 在 GitHub 上创建一个 [Personal access token (classic)](https://github.com/settings/tokens)，需要选中 `write:packages` 和 `delete:packages` 权限。

2. 登录到 GitHub Container Registry

   ```sh
   docker login ghcr.io
   ```

   > 密码请使用你刚刚创建的令牌。

3. 标记镜像并推送到 GitHub Container Registry（ghcr.io）

   ```sh
   docker tag <image>:<tag> ghcr.io/<username>/<image>:<tag>
   docker push ghcr.io/<username>/<image>:<tag>
   ```

参考：[使用容器注册表 | GitHub Docs](https://docs.github.com/zh/packages/working-with-a-github-packages-registry/working-with-the-container-registry)

#### 将镜像连接到 GitHub 仓库

在 GitHub 的 Profile 中，选择 Packages，找到刚刚推送的镜像，点击 `Connect to a repository`。

或者，编辑镜像的 Dockerfile，加入以下内容：

```Dockerfile
LABEL org.opencontainers.image.source https://github.com/<username>/<repo>
```

> 将 `<repo>` 替换为你要推送的库