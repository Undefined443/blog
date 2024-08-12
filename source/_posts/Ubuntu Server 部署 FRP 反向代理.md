## 踩坑记录

我使用的配置文件是官方提供的示例配置文件 [通过 SSH 访问内网机器](https://gofrp.org/zh-cn/docs/examples/ssh/)，应该没有问题。

第一次我使用 Docker 镜像 [snowdreamtech/frps](https://hub.docker.com/r/snowdreamtech/frps) 在服务器上部署 frps，发现始终连不上去。在内网机器的 frpc log 中显示如下错误：

```log
2024-03-12 17:02:31 2024/03/12 09:02:31 [I] [root.go:142] start frpc service for config file [/etc/frp/frpc.toml]
2024-03-12 17:02:31 2024/03/12 09:02:31 [I] [service.go:287] try to connect to server...
2024-03-12 17:02:31 2024/03/12 09:02:31 [W] [service.go:290] connect to server error: dial tcp 8.134.175.243:7000: connect: connection refused
2024-03-12 17:02:31 2024/03/12 09:02:31 [I] [root.go:160] frpc service for config file [/etc/frp/frpc.toml] stopped
2024-03-12 17:02:31 login to the server failed: dial tcp 8.134.175.243:7000: connect: connection refused. With loginFailExit enabled, no additional retries will be attempted
```

可以看到连接失败的原因是连接 7000 端口时遇到了 `connection refused` 错误。这说明服务器的 7000 端口可能没有打开。

而在服务器的 frps log 中则一切正常：

```log
2024/03/12 09:01:53 [I] [root.go:105] frps uses config file: /etc/frp/frps.toml
2024/03/12 09:01:54 [I] [service.go:225] frps tcp listen on 0.0.0.0:7000
2024/03/12 09:01:54 [I] [root.go:114] frps started successfully
```

可以看到 frps 正在监听 7000 端口。

一开始我以为是服务器的防火墙没开启 7000 端口，可是后来发现和防火墙设置没关系。后来我在服务器上查看 7000 端口的使用情况：

```sh
sudo lsof -i :7000
```

输出为空。

这说明没有服务在监听 7000 端口，那我的内网主机当然不可能连的上。

于是我在服务器上使用 brew 安装了一个 frps，并使用它部署了 frps 服务：

```sh
brew install frps
brew services start frps
```

最后内网 frpc 连接成功了：

```log
2024-03-12 17:06:25 2024/03/12 09:06:25 [I] [root.go:142] start frpc service for config file [/etc/frp/frpc.toml]
2024-03-12 17:06:25 2024/03/12 09:06:25 [I] [service.go:287] try to connect to server...
2024-03-12 17:06:25 2024/03/12 09:06:25 [I] [service.go:279] [2d0f725bae7e4407] login to server success, get run id [2d0f725bae7e4407]
2024-03-12 17:06:25 2024/03/12 09:06:25 [I] [proxy_manager.go:173] [2d0f725bae7e4407] proxy added: [ssh]
2024-03-12 17:06:25 2024/03/12 09:06:25 [I] [control.go:170] [2d0f725bae7e4407] [ssh] start proxy success
```

可以看到内网 frpc 已经建立了 proxy。

然而这时又出了问题：我在外网使用 SSH 连接内网机器依旧不成功。

于是我在内网机器上也使用 brew 安装了 frpc，再次尝试连接。

这次连接成功了。

经过查阅 Docker host 网络模式参考文档，发现原因出在 host 网络模式上。我在服务器和内网机器上使用的都是 Docker Desktop 而不是 Docker CE，而此时的 Docker Desktop 版本还不支持 host 网络模式。因此我使用 Docker Desktop 部署在 host 网络模式下的容器是无法使用主机网络的。

2024.4.20 更新：目前 Docker Desktop 4.29 在 Windows 和 macOS 上的版本已经支持了 host 网络模式，而 Linux 版本则正处于实验阶段。所以现在使用 Docker Desktop 运行 host 网络模式的容器应该不会再出问题。

> Host networking is also supported on Docker Desktop version 4.29 and later for Mac, Windows, and Linux as a beta feature. To enable this feature, navigate to the Features in development tab in Settings, and then select Enable host networking.

参见 Docker host 网络模式参考文档：[Host network driver | Docker Docs](https://docs.docker.com/network/drivers/host/#docker-desktop)