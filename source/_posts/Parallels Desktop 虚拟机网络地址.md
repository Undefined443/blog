- `bridge100` 是宿主机在共享网络中的地址
- `bridge101` 是宿主机在 Host-Only 网络中的地址

## 查询宿主机 IP 地址

```sh
$ ip addr show
# 共享网络（默认）
bridge100: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	ether ca:89:f3:ae:ab:64
	inet 10.211.55.2/24 brd 10.211.55.255 bridge100
	inet6 fe80::c889:f3ff:feae:ab64/64 scopeid 0x17
	inet6 fdb2:2c26:f4e4::1/64
# Host-Only
bridge101: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	ether ca:89:f3:ae:ab:65
	inet 10.37.129.2/24 brd 10.37.129.255 bridge101
	inet6 fe80::c889:f3ff:feae:ab65/64 scopeid 0x19
	inet6 fdb2:2c26:f4e4:1::1/64
# TODO
bridge102: flags=8a63<UP,BROADCAST,SMART,RUNNING,ALLMULTI,SIMPLEX,MULTICAST> mtu 1500
	ether ca:89:f3:ae:ab:66
	inet 192.168.64.1/24 brd 192.168.64.255 bridge102
	inet6 fe80::c889:f3ff:feae:ab66/64 scopeid 0x1c
	inet6 fdcc:924:7645:df47:4ea:ff37:8095:dc31/64 autoconf secured
```

## 查询虚拟机 IP 地址

> 以下命令均在宿主机上执行

可以在虚拟机列表中找到虚拟机的 IP 地址：

```sh
prlctl list -f
```

或者在虚拟机上执行查询命令：

```sh
prlctl exec 'VM_ID|VM_NAME' ipconfig      # Windows
prlctl exec 'VM_ID|VM_NAME' ip addr show  # Linux
```

参考：[How to get the IP address used by a Parallels VM from the host? | Stack Overflow](https://stackoverflow.com/questions/17896984/how-to-get-the-ip-address-used-by-a-parallels-vm-from-the-host)