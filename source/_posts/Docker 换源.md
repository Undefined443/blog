⚠️ 注意：该笔记中的镜像源已经过时，最新的可用镜像源列表请参考 [Docker Hub 镜像加速器 | GitHub](https://gist.github.com/y0ngb1n/7e8f16af3242c7815e7ca2f0833d3ea6)

## Docker Desktop 换源

打开 Docker Desktop，在 Settings > Docker Engine 中，添加如下内容：

```json
  "registry-mirrors": [
    "https://registry.docker-cn.com",
    "https://docker.mirrors.ustc.edu.cn",
    "http://hub-mirror.c.163.com"
  ]
```

> - 别忘了在上一个条目的末尾加上逗号 `,`
>
> - ⚠️ 注意：错误的配置项会导致 Docker 无法启动。如果 Docker 无法启动，请在 Docker 后台图标上右键 > Troubleshoot > Reset to factory defaults

修改完成后的配置如下：

![image](https://s2.loli.net/2024/07/06/G4NueP2ZzA6tQCn.png)

使用 `docker info` 命令检查设置是否生效：

```sh
$ docker info
...
 Registry Mirrors:
  https://registry.docker-cn.com/
  https://docker.mirrors.ustc.edu.cn/
  http://hub-mirror.c.163.com/
...
```

看到 `Registry Mirrors` 条目下有我们设置的镜像源，说明镜像设置成功了。

## Docker CE 换源

1. 编辑 `/etc/docker/daemon.json`

   ```sh
   sudo vim /etc/docker/daemon.json
   ```

2. 向 `/etc/docker/daemon.json` 添加如下内容：

   ```json
   {
     "registry-mirrors": [
       "https://registry.hub.docker.com",
       "https://mirror.gcr.io",
       "https://registry-1.docker.io"
     ]
   }
   ```

3. 重新加载配置文件，并重启 docker 服务

   ```sh
   sudo systemctl daemon-reload  # 重新加载配置文件
   sudo systemctl restart docker  # 重启 docker 服务
   ```

镜像源参考 [镜像加速器 | Docker 从入门到实践 - yeasy](https://yeasy.gitbook.io/docker_practice/install/mirror)

镜像源测速脚本：[Docker-Mirror-Benchmark | GitHub](https://github.com/Undefined443/Docker-Mirror-Benchmark)

---

已知不可用镜像：

- Docker 中国区官方镜像：https://registry.docker-cn.com
- 网易镜像源：http://hub-mirror.c.163.com
- 百度云镜像源：https://mirror.baidubce.com
- 中科大镜像源：https://docker.mirrors.ustc.edu.cn

建议参考：[解决目前 Docker Hub 国内无法访问方法汇总 | 知乎](https://zhuanlan.zhihu.com/p/642560164)