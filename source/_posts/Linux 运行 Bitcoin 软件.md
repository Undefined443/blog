首先进入官网 [bitcoin.org](https://bitcoin.org/zh_CN/download) 下载 Bitcoin Core。

下载得到 tar.gz 文件后解压，并安装：

```sh
tar xzf bitcoin-25.0-x86_64-linux-gnu.tar.gz
sudo install -m 0755 -o root -g root -t /usr/local/bin bitcoin-25.0/bin/*
```

使用 `bitcoin-qt` 命令打开 Bitcoin Core 图形界面。

如果提示 `libxcb-xinerama` 未安装，则使用 `sudo apt install libxcb-xinerama0` 命令安装。

第一次启动 Bitcoin Core 时会同步区块。由于网络原因，同步区块的速度为 0。我们需要先设置代理。打开 Settings > Options > Network，选中 Connect through SOCKS5 proxy (default proxy)，并设置代理服务器。

![image](https://s2.loli.net/2024/07/07/s7GmC5lDhEAMOwU.png)

重启软件，就可以看到已经开始同步区块了。

![image](https://s2.loli.net/2024/07/07/uQAc1WySqrhto97.png)

参考：[Running A Full Node | bitcoin.org](https://bitcoin.org/en/full-node)