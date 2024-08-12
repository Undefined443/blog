## 服务端

### 编写配置文件

```sh
vim ~/.config/frp/frps.toml
```

```toml
bindPort = 7000

# Web Dashboard
[webServer]
addr = "0.0.0.0"
port = 7500
user = "xxx"
password = "xxx"
```

### 启动 Docker 容器

```sh
docker run -d --restart always --network host --name frps -v ~/.config/frp/frps.toml:/etc/frp/frps.toml snowdreamtech/frps
```

## 客户端

### 编写配置文件

```sh
vim ~/.config/frp/frpc.toml
```

```toml
serverAddr = "x.x.x.x"
serverPort = 7000

# 认证
[auth]
method = "token"
token = "xxx"

# Web Dashboard
[webServer]
addr = "0.0.0.0"
port = 7400
user = "xxx"
password = "xxx"
pprofEnable = false

# 内网穿透配置
[[proxies]]
name = "ssh"
type = "tcp"
localIP = "127.0.0.1"
localPort = 22
remotePort = 6000
```

### 启动 Docker 容器

```sh
docker run -d --restart always --network host --name frpc -v ~/.config/frp/frpc.toml:/etc/frp/frpc.toml snowdreamtech/frpc
```

查看日志：

```sh
docker logs frpc
```