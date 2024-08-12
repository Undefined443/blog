### 使用 [lsof](https://www.linux-man.cn/command/?command=lsof)

```sh
$ sudo lsof -i :22
COMMAND    PID USER   FD   TYPE  DEVICE SIZE/OFF NODE NAME
sshd       963 root    3u  IPv4   21816      0t0  TCP *:ssh (LISTEN)
sshd    292636 root    4u  IPv4 3082489      0t0  TCP iZ7xvhevxjvf4dzop816ukZ:ssh->113.140.11.124:55978 (ESTABLISHED)
sshd    292692 xiao    4u  IPv4 3082489      0t0  TCP iZ7xvhevxjvf4dzop816ukZ:ssh->113.140.11.124:55978 (ESTABLISHED)
sshd    292957 root    4u  IPv4 3082136      0t0  TCP iZ7xvhevxjvf4dzop816ukZ:ssh->113.140.11.124:55979 (ESTABLISHED)
sshd    293033 xiao    4u  IPv4 3082136      0t0  TCP iZ7xvhevxjvf4dzop816ukZ:ssh->113.140.11.124:55979 (ESTABLISHED)
```

### 使用 [netstat](https://www.linux-man.cn/command/?command=netstat)

```sh
$ sudo netstat -tulnp | grep ':22'
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      963/sshd: /usr/sbin
```

### 使用 [ss](https://www.linux-man.cn/command/?command=ss)

ss 是一种比 netstat 性能更高的工具

```sh
$ sudo ss -tulnp | grep ':22'
tcp   LISTEN 0      128                0.0.0.0:22         0.0.0.0:*    users:(("sshd",pid=963,fd=3))
```

- `t`: 显示 TCP 套接字使用情况
- `u`: 显示 UDP 套接字使用情况
- `l`: 只显示正在监听的端口
- `n`: 不解析服务名称，比如不把 22 端口显示为 ssh
- `p`: 显示正在使用套接字的进程

### 使用 [fuser](https://www.linux-man.cn/command/?command=fuser)

```sh
$ sudo fuser 22/tcp
22/tcp:                963 292636 292692 292957 293033
```