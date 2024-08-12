## SSH 常用命令

```sh
# 创建 SSH 密钥
ssh-keygen -t rsa
# 登录远程主机
ssh USER@HOST
# 配置无密码登录
ssh-copy-id USER@HOST
# 使用 SCP 传输文件
scp FILE USER@HOST:PATH # 使用 scp -r 递归拷贝整个目录
```

## SSH Config 文件

可以简化连接命令

`~/.ssh/config`:

```sh
Host *
    StrictHostKeyChecking no
    UserKnownHostsFile /dev/null

    # 保活
    ServerAliveInterval 120

    # TCP 连接复用
    ControlMaster auto
    ControlPath ~/.ssh/multiplex/%r@%h:%p
    ControlPersist 1

    # 设置连接到任意主机使用的密钥（默认为 id_rsa）
    IdentityFile ~/.ssh/id_rsa


Host example
  HostName example.com
  User username
  IdentityFile ~/.ssh/private_key.pem
  Port 2222


# 设置通过 443 端口连接到 github.com
Host github.com
  HostName ssh.github.com
  User git
  Port 443
```

登录时可以使用 `ssh example` 代替 `ssh username@example.com -p 2222`

**说明**

`scp` 只能识别以正斜杠分隔的路径，即便是在 Windows 上也是如此。在 Shell 中的路径：`/Users/xiao/.ssh/authorized_keys` 相当于 CMD 上的：`C:\Users\xiao\.ssh\authorized_keys`

## 允许以 root 身份登录

编辑 SSH 配置文件：

```sh
sudo vim /etc/ssh/sshd_config
```

找到这两条命令：

```sh
#PermitRootLogin prohibit-password
#PermitEmptyPasswords no
```

改成这两条：

```sh
PermitRootLogin yes
PermitEmptyPasswords yes
```

最后重启 `sshd` 服务：

```sh
sudo systemctl restart ssh
```

## SSH 连接

```sh
ssh USER@HOST
```

[使用 SSH 连接到 Windows](https://www.cnblogs.com/Undefined443/p/17978984)

[Windows 使用连接到其他主机](https://www.cnblogs.com/Undefined443/p/18146421)