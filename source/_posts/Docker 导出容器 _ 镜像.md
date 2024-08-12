有时由于镜像大小、网络限制等原因，我们不能将本地制作的容器 / 镜像上传到公共容器注册表。此时我们可以选择将镜像以本地文件的形式导出。

## 导入 / 导出容器

```sh
docker export "CONTAINER" > image.tar  # 将容器导出为镜像文件
docker import - "IMAGE" < image.tar    # 导入镜像文件为镜像
```

参考：

- [docker container export | Docker Docs](https://docs.docker.com/reference/cli/docker/container/export/)
- [docker image import | Docker Docs](https://docs.docker.com/reference/cli/docker/image/import/)

## 导入 / 导出镜像

```sh
docker save "IMAGE" > image.tar  # 导出镜像
docker load < image.tar          # 导入镜像
```

你还可以使用 gzip 压缩导出的容器或镜像文件：

```sh
docker save "IMAGE" | gzip > image.tar.gz  # 导出镜像
docker load < image.tar.gz                 # 导出镜像
```

参考：

- [docker image save | Docker Docs](https://docs.docker.com/reference/cli/docker/image/save/)
- [docker image load | Docker Docs](https://docs.docker.com/reference/cli/docker/image/load/)

## 将容器保存为镜像

```sh
docker commit \
    -m "What you did to the image" \
    -a "Author Name" \
    <container> <image>
```

参考：[Committing Changes in a Container to a Docker Image](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04#step-7-committing-changes-in-a-container-to-a-docker-image)