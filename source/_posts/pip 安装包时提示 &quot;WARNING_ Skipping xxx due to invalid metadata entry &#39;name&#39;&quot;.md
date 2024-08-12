我最近在使用 `pip` 安装包的时候经常遇到如下警告：

```
WARNING: Skipping /opt/homebrew/lib/python3.11/site-packages/numpy-1.26.3.dist-info due to invalid metadata entry 'name'
WARNING: Skipping /opt/homebrew/lib/python3.11/site-packages/protobuf-4.25.2-py3.11.egg-info due to invalid metadata entry 'name'
```

经过一番搜索，我发现问题的原因是由于 Python 的 `site-packages` 文件夹中存在空目录。以下命令将修复此问题：

```sh
find '/opt/homebrew/lib/python3.11/site-packages' -type d -empty -delete
```

这将删除 `site-packages` 文件夹下的空目录。记得将命令中 `site-packages` 的目录换成你实际的目录。

参考：[pip has problems with metadata | Stack Overflow](https://stackoverflow.com/questions/67074684/pip-has-problems-with-metadata)