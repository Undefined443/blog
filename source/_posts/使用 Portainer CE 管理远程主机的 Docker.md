## Prerequisites

你已经在本地主机安装了 Portainer CE

## 安装

打开本地主机的 Portainer CE 界面，默认地址为 `localhost:9443`

在左侧边栏中找到 `Environments`，进入并点击 `Add environments`。

选择 Docker Standalone，然后点击下面的 `Start Wizard`

![image](https://s2.loli.net/2024/07/07/C9ZzIrjlxo84OM6.png)

接下来的连接模式选择 `Agent`，然后复制它给你的命令，并将命令粘贴到你要管理的**远程主机**中运行。

> 在远程主机运行命令时，你可能会遇到如下错误：
>
> ```
> docker: Error response from daemon: error while creating mount source path '/var/lib/docker/volumes': mkdir /var/lib/docker/volumes: permission denied.
> ```
>
> 这时你需要在命令前加上 `sudo` 并再次运行。

![image](https://s2.loli.net/2024/07/07/bMIzG7NVcEUOF85.png)

远程主机的命令运行完成后，在这里给你要管理的远程主机填写名字（Name）以及远程主机的地址（Environment address）。例如，地址可以是：`192.168.1.2:9001`。这里的端口号取决于你之前运行的命令中指定的端口号。

填写完成后，点击页面底部的 `Connect`。

此时，如果一切顺利的话，你会在页面右上角看到环境添加成功的提示。