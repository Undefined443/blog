### 本地转发

```sh
ssh USER@HOST -CNfL 80:localhost:8080
```

此时访问本地主机的 80 端口将会被转发到远程主机 HOST 的 localhost:8080 端口。

- `-L`: 本地转发
- `-C`: 此标志启用压缩，有助于减少资源消耗并加快速度。
- `-N`: 不发送任何命令，只用来建立连接。
- `-f`: 将 SSH 连接放到后台。避免长时间不用 SSH 连接时终端失去响应

可以通过 `ps` 命令找到 ssh 进程并通过 `kill` 命令来关闭隧道。

#### 配置文件

可以通过配置文件来简化隧道的建立。编辑 `~/.ssh/config`：

```ssh
Host local-forward
    HostName example.com
    User my_user
    IdentityFile ~/.ssh/my_private_key
    LocalForward 80 localhost:8080
    Compression yes
    ServerAliveInterval 60
    ServerAliveCountMax 2
```

- `Host local-forward`：这是这个连接配置的别名，用在`ssh`命令中作为目标主机的参数。
- `HostName example.com`：实际主机名或 IP 地址。
- `User my_user`：远程服务器的用户名。
- `IdentityFile ~/.ssh/my_private_key`：用于身份验证的私钥文件路径。
- `LocalForward 80 localhost:8080`：这是隧道配置的关键部分。它会将本地机器上的 80 端口转发到远程机器上的 8080 端口。当你在本地机器上访问 localhost 的 80 端口时，实际上是通过 SSH 隧道访问远程机器上的 8080 端口。
- `Compression yes`: 启用数据压缩。
- `ServerAliveInterval 60`：每 60 秒向服务器发送一次保活消息。
- `ServerAliveCountMax 2`：在认定服务器不响应之前，允许发送保活消息的最大次数。

### 远程转发

```sh
ssh USER@HOST -R 80:localhost:8080
```

此时访问远程主机 HOST 的 80 端口会被转发到本地主机的 localhost:8080 端口。

#### 配置文件

```ssh
Host reverse-forward
    HostName example.com
    User my_user
    RemoteForward 80 localhost:8080
```

- `RemoteForward 80 localhost:8080`：配置远程端口转发。这条命令的意思是将远程服务器的 80 端口转发到本机的 localhost:8080 端口。访问远程服务器的 80 端口就像访问本地的 localhost:80 端口一样。

这个配置意味着，当你连接到 `remote-forward` 这个配置指定的主机时，SSH 会自动设置一个远程端口转发，把远程主机的 80 端口映射到本机的 8080 端口。

### 动态转发

```sh
ssh USER@HOST -D 7890
```

此时如果在应用程序中配置代理服务器为 `socks5://HOST:7890`，则所有请求都将被发送到远程主机的 7890 端口进行代理。

#### 配置文件

```ssh
   Host dynamic-forward
       HostName example.com
       User my_user
       DynamicForward 7890
   ```

   - `DynamicForward 1080`: 启用动态端口转发，并绑定本地的 1080 端口。