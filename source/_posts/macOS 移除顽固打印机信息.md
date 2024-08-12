### 问题描述

当我打开 Parallels Desktop 的 Ubuntu 虚拟机时，总是会看到打印机已添加的提示：

![image](https://s2.loli.net/2024/07/07/2FDHzM8rpNTUbhu.png)

### 查看已有打印机信息

```sh
$ lpstat -p
打印机Lenovo_M7206W闲置，启用时间始于五  1/ 6 23:54:09 2023
```

这里可以看到打印机的名字是 `Lenovo_M7206W`

### 查看打印机具体信息

使用 `lpoptions` 查看打印机具体信息：

```sh
$ lpoptions -p Lenovo_M7206W
copies=1 device-uri=usb://Lenovo/M7206W?serial=00000WP01919168 finishings=3 job-cancel-after=10800 job-hold-until=no-hold job-priority=50 job-sheets=none,none marker-change-time=0 number-up=1 printer-info='Lenovo M7206W' printer-is-accepting-jobs=true printer-is-shared=false printer-is-temporary=false printer-location='MacBook Pro' printer-make-and-model='Local Raw Printer' printer-state=3 printer-state-change-time=1673020449 printer-state-reasons=offline-report printer-type=77594628 printer-uri-supported=ipp://localhost/printers/Lenovo_M7206W
```

### 移除打印机

我们可以使用 `lpadmin` 命令移除打印机：

```sh
lpadmin -x Lenovo_M7206W
```

运行完成后，我们再次运行 `lpstat -p` 查看已有打印机信息：

```sh
$ lpstat -p
lpstat: 未添加目的位置。
```

`/var/spool/cups/` 目录下可能还有一些残留文件：

```sh
sudo rm -rf /var/spool/cups/cache
sudo rm -rf /var/spool/cups/tmp
```

可以看到我们的打印机已被移除。

再次打开 Ubuntu 虚拟机时，也不会看到打印机已添加的信息。