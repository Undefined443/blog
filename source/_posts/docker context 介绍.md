```
$ docker context ls
NAME            DESCRIPTION                               DOCKER ENDPOINT                                ERROR
default         Current DOCKER_HOST based configuration   unix:///var/run/docker.sock
desktop-linux   Docker Desktop                            unix:///Users/user/.docker/run/docker.sock
orbstack *      OrbStack                                  unix:///Users/user/.orbstack/run/docker.sock
```

- `default` 是 Docker Engine 使用的上下文
- `desktop-linux` 是 Docker Desktop 使用的上下文
- `orbstack` 是 OrbStack 使用的上下文

不同上下文中的镜像、容器是相互隔离的。即，Docker Engine (CLI) 中的镜像和容器在 Docker Desktop (GUI) 中是看不到的。