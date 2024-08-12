此文档参考官方文档 [Install Portainer CE with Docker on Linux](https://docs.portainer.io/start/install-ce/server/docker/linux) 编写。

1. 创建容器
    ```sh
    docker volume create portainer_data
    ```

2. 启动 Portainer CE
    ```sh
    docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
    ```

接下来，你就可以在 `https://localhost:9443` 管理你的 Docker 了。

如果你需要使用 Portainer 管理远程主机上的 Docker，请参阅：[使用 Portainer CE 管理远程主机的 Docker](https://www.cnblogs.com/Undefined443/p/18069336)