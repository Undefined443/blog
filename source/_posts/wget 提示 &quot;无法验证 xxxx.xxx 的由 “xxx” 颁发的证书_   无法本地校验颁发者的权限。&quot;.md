有一天在使用 wget 下载文件时，出现了无法验证证书的提示：

```sh
$ wget https://github.com/zayronxio/Mkos-Big-Sur/releases/download/0.3/Mkos-Big-Sur.tar.xz
--2024-04-14 03:57:50--  https://github.com/zayronxio/Mkos-Big-Sur/releases/download/0.3/Mkos-Big-Sur.tar.xz
正在连接 127.0.0.1:6152... 已连接。
错误: 无法验证 github.com 的由 “CN=Sectigo ECC Domain Validation Secure Server CA,O=Sectigo Limited,L=Salford,ST=Greater Manchester,C=GB” 颁发的证书:
  无法本地校验颁发者的权限。
要以不安全的方式连接至 github.com，使用“--no-check-certificate”。
```

出现问题的原因是计算机上缺少了必要的根 CA 证书，重新安装 CA 证书包即可解决。

macOS:

```sh
brew reinstall ca-certificates
```

Ubuntu:

```sh
sudo apt reinstall ca-certificates
```