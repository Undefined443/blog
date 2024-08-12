### Linux

显示网络设备的状态：

```sh
$ nmcli device status
DEVICE   TYPE      STATE                   CONNECTION         
enp0s5   ethernet  connected               Wired connection 1 
lo       loopback  unmanaged               --                 
```

这里我们看到有两个网络设备：`enp0s5` 和 `lo`。

- `enp0s5` 是 `ethernet`（以太网）
- `lo` 是 `loopback`（环回接口）

我们要找的是以太网接口的 IP 地址。在这里是 `enp0s5`.

查看 `enp0s5` 接口的具体信息：

```sh
$ ip addr show enp0s5
2: enp0s5: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 00:1c:42:9b:48:d6 brd ff:ff:ff:ff:ff:ff
    inet 10.211.55.6/24 brd 10.211.55.255 scope global dynamic noprefixroute enp0s5
       valid_lft 1007sec preferred_lft 1007sec
    inet6 fdb2:2c26:f4e4:0:b3fc:ed85:dcfb:317f/64 scope global temporary dynamic 
       valid_lft 604009sec preferred_lft 85336sec
    inet6 fdb2:2c26:f4e4:0:3e08:a4fd:b176:a427/64 scope global dynamic mngtmpaddr noprefixroute 
       valid_lft 2591859sec preferred_lft 604659sec
    inet6 fe80::42f1:b7f7:4114:647b/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```

### macOS

查看所有网络接口：

```sh
$ networksetup -listallhardwareports

Hardware Port: Ethernet Adapter (en4)
Device: en4
Ethernet Address: 4a:d7:f9:4e:a8:e6

Hardware Port: Ethernet Adapter (en5)
Device: en5
Ethernet Address: 4a:d7:f9:4e:a8:e7

Hardware Port: Ethernet Adapter (en6)
Device: en6
Ethernet Address: 4a:d7:f9:4e:a8:e8

Hardware Port: Thunderbolt Bridge
Device: bridge0
Ethernet Address: 37:d3:98:60:9c:00

Hardware Port: Wi-Fi
Device: en0
Ethernet Address: d8:89:f4:ea:59:3e

Hardware Port: Thunderbolt 1
Device: en1
Ethernet Address: 37:d3:98:60:9c:00

Hardware Port: Thunderbolt 2
Device: en2
Ethernet Address: 37:d3:98:60:9c:04

Hardware Port: Thunderbolt 3
Device: en3
Ethernet Address: 37:d3:87:60:9c:08

VLAN Configurations
===================
```

这里看到我们的 mac 上有多个接口：`Ethernet Adapter`、`Thunderbolt Beidge`、`Wi-Fi`、`Thunderbolt`。我的 mac 当前是在连接 Wi-Fi 使用，因此我们要查看 `Wi-Fi` 接口 `en0` 的 IP 地址。

查看 `en0` 接口的具体信息：

```sh
$ ip addr show en0
en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	ether c8:89:f3:ea:59:3e
	inet6 fe80::c6c:28b5:9e91:4dc5/64 secured scopeid 0xf
	inet 10.198.159.58/16 brd 10.198.255.255 en0
```