参考：[Jupyter Docker Stacks documentation](https://jupyter-docker-stacks.readthedocs.io/en/latest/)

容器地址在 [quay.io/jupyter/scipy-notebook](https://quay.io/jupyter/scipy-notebook)

如果你直接运行命令：

```sh
docker run -p 10000:8888 quay.io/jupyter/scipy-notebook:2024-03-14
```

你启动的 Jupyter 服务会运行在一个奇怪的域名：

```
To access the server, open this file in a browser:
        file:///home/jovyan/.local/share/jupyter/runtime/jpserver-7-open.html
    Or copy and paste one of these URLs:
        http://1f6fc664b015:8888/lab?token=73a9a4986e03cf7bec47b0a93de0bb712fe2d1cefbb02552
        http://127.0.0.1:8888/lab?token=73a9a4986e03cf7bec47b0a93de0bb712fe2d1cefbb02552
```

你可以设置 `ip='*'` 参数来将域名变成 `localhost`：

```sh
docker run -p 10000:8888 quay.io/jupyter/scipy-notebook:2024-03-14 start-notebook.py --ServerApp.ip='*'
```

```
To access the server, open this file in a browser:
        file:///home/jovyan/.local/share/jupyter/runtime/jpserver-7-open.html
    Or copy and paste one of these URLs:
        http://localhost:8888/lab?token=f0bfa879fbbd9859579ee7e1cf2b166b600f912feb8f62af
        http://127.0.0.1:8888/lab?token=f0bfa879fbbd9859579ee7e1cf2b166b600f912feb8f62af
```

> 这里 `ip` 参数是通过 `start-notebook.py` 脚本提供的。向脚本指定 `--ServerApp.ip` 选项来设置 `ip` 参数。

你还可以设置容器名、后台运行、关联挂载、登录密码等参数：

```sh
docker run \
    --name jupyter \
    --rm \
    -dp 8888:8888 \
    -v ~/.notebooks:/home/jovyan \
    -e JUPYTER_PASSWORD=YOUR_PASSWORD \
    quay.io/jupyter/scipy-notebook start-notebook.py --ServerApp.ip='*'
```

> 将 `YOUR_PASSWORD` 替换为你的密码

然后通过下面的命令来查看服务地址：

```sh
docker exec jupyter jupyter server list
```

```
Currently running servers:
http://localhost:8888/?token=03bd71def07b54f61399f03b8214c19f8ac2d51dd614707d :: /home/jovyan
```

Jupyter 镜像层级关系：

![](https://jupyter-docker-stacks.readthedocs.io/en/latest/_images/inherit.svg)