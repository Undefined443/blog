在 Ubuntu 上安装完 PDI 后启动 `spoon.sh` 时提示安装 `libwebkitgtk-1.0-0`。由于 apt 官方源中不包含此软件包，因此要添加该软件包的源，以及源对应的 gpg 公钥，然后再使用 apt 进行安装。

```
WARNING:  no libwebkitgtk-1.0 detected, some features will be unavailable
    Consider installing the package with apt-get or yum.
    e.g. 'sudo apt-get install libwebkitgtk-1.0-0'
```

#### 添加新源

```sh
sudo vim /etc/apt/sources.list  # 编辑 APT 源列表
```

在文件中添加新的一行：

```
deb http://cz.archive.ubuntu.com/ubuntu bionic main universe
```

接下来添加 gpg 公钥：

```sh
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 40976EAF437D05B5
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 3B4FE6ACC0B21F32
```

最后更新 APT 索引：

```sh
sudo apt-get update
```

- [LIBWEBKITGTK-1.0-0 ON UBUNTU 22.04 LTS | Pentaho Community](https://community.hitachivantara.com/discussion/libwebkitgtk-10-0-on-ubuntu-2204-lts)

- [Chris Jean: Fix apt-get update "the following signatures couldn’t be verified because the public key is not available"](https://chrisjean.com/fix-apt-get-update-the-following-signatures-couldnt-be-verified-because-the-public-key-is-not-available/)