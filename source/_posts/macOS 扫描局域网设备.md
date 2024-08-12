### arp-scan:

arp-scan 是一个直接用于扫描本地网络中的设备的 UNIX 工具。这不是 macOS 自带的，但可以使用 Homebrew 安装。首先安装 Homebrew（如果尚未安装），然后通过终端运行以下命令来安装 arp-scan：

```sh
brew install arp-scan
```

使用 `arp-scan` 扫描局域网：

```sh
sudo arp-scan --interface=en0 --localnet
```

你需要根据你的网络接口替换 `en0`。`en0` 通常是有线连接，而 `en1` 或 `en2` 可能是无线连接。

以下为命令执行结果：

> PS: 在执行以下命令时，我的网络环境是无线连接，但命令中使用的网络接口为 `en0`

```
Interface: en0, type: EN10MB, MAC: c9:89:f4:ea:58:3a, IPv4: 192.168.0.100
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.0.1	48:5f:08:f1:f9:55	(Unknown)
192.168.0.104	14:9b:f3:f4:94:93	GUANGDONG OPPO MOBILE TELECOMMUNICATIONS CORP.,LTD
192.168.0.107	98:40:bb:1c:4f:cc	Dell Inc.
192.168.0.101	be:aa:e2:70:fb:d2	(Unknown: locally administered)
192.168.0.103	12:93:c5:ca:ca:be	(Unknown: locally administered)
192.168.0.105	92:42:ce:c3:f1:d6	(Unknown: locally administered)
192.168.0.102	1a:e8:e7:79:61:74	(Unknown: locally administered)

4573 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.858 seconds (137.78 hosts/sec). 7 responded
```

### nmap:

`nmap` 是一个功能强大的网络扫描工具，可用于设备发现、端口扫描等。这个工具也不是 macOS 默认安装的，但同样可以通过 Homebrew 安装：

> `nmap` 的扫描速度较慢

```sh
brew install nmap
```

使用 `nmap` 扫描本地网络可能的所有 IP 地址：

```sh
sudo nmap -sn 192.168.1.0/24
```

请将 `192.168.1.0/24` 替换为您的实际局域网 IP 范围。`-sn` 参数表示进行 ping 扫描（即不进行端口扫描）。

以下为命令执行结果：

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-02-13 00:23 CST
Nmap scan report for 192.168.0.1
Host is up (0.012s latency).
Nmap scan report for 192.168.0.100
Host is up (0.00032s latency).
Nmap scan report for 192.168.0.101
Host is up (0.050s latency).
Nmap scan report for 192.168.0.102
Host is up (0.056s latency).
Nmap scan report for 192.168.0.103
Host is up (0.032s latency).
Nmap scan report for 192.168.0.105
Host is up (0.059s latency).
Nmap done: 256 IP addresses (6 hosts up) scanned in 13.52 seconds
```

请注意，扫描网络可能影响网络性能，也可能违反网络使用协议。在商业或教育机构的网络上运行扫描之前，请确保您有权限这样做。