有时候由于配置的失误，导致构建了好久的镜像没能推送到云或者保存到本地。而如果重新构建，则可能又要全部重来。其实这时候我们可以导出 buildx 中的缓存到本地文件，再将本地文件导入为镜像。这样可以节省不必要的等待时间。


```sh
# 抓取构建缓存中的镜像并为其创建一个新的标签
docker buildx imagetools create myimage:latest -t newtag:latest

# 保存镜像到文件
docker save newtag:latest -o myimage.tar

# 从文件载入镜像
docker load -i myimage.tar
```