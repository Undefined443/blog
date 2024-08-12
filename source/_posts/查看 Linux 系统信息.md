## 查看系统信息

### 查看发行版信息

```sh
cat /etc/os-release
lsb_release -a
```

### 查看公网 IP 地址

```sh
curl -4 icanhazip.com
```

### 查看系统架构

```sh
uname -m  # POSIX 标准
arch      # GNU Coreutils
```

### 查看硬件信息

```sh
sudo apt install -y inxi  # 需要先安装使用
```

```sh
inxi     # 查看 CPU、内存等信息
inxi -G  # 查看显卡信息
```

#### 查看 CPU / GPU 信息

cpufetch

```sh
sudo apt install -y cpufetch
cpufetch
```

gpufetch

```sh
sudo apt install zlib1g-dev cmake make gcc g++
git clone https://github.com/Dr-Noob/gpufetch
cd gpufetch
./build.sh
./gpufetch
```

#### 查看系统运行情况

```sh
htop     # 终端任务管理器
btop     # 更加强大的终端任务管理器
free -h  # 查看内存使用情况
```

## 系统性能测试

### CPU 性能测试

```sh
sudo apt install sysbench
```

```sh
sysbench --test=cpu --cpu-max-prime=20000 run
```

### 内存性能测试

```sh
sysbench --test=memory --memory-block-size=1M --memory-total-size=10G run
```

### 磁盘 I/O 性能测试

测试磁盘的响应时间：

```sh
ioping -c 10 .
```

这条命令测试当前目录的磁盘响应时间，执行 10 次。

测试磁盘写入速度：

```sh
dd if=/dev/zero of=testfile bs=1G count=1 oflag=dsync
```

这条命令使用 `dd` 从 `/dev/zero` 写入 1GB 数据到 `testfile` 文件，`oflag=dsync` 确保直接写入磁盘。

### 网络性能测试

使用 Speedtest：

```sh
curl -s https://packagecloud.io/install/repositories/ookla/speedtest-cli/script.deb.sh | sudo sh
sudo apt install speedtest
```

```sh
speedtest
```

使用 `iperf3`：

`iperf3` 是一个网络性能测试工具，主要用于测量两个主机之间的最大 TCP 和 UDP 带宽性能。

```sh
sudo apt install iperf3
```

在测试之前，需要在一台机器上启动 `iperf3` 作为服务器：

```sh
iperf3 -s
```

然后在另一台机器上执行客户端模式，连接到服务器：

```sh
iperf3 -c [服务器IP地址]
```