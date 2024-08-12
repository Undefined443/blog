## 安装 doctl

### macOS

```sh
brew install doctl
```

### Ubuntu

```sh
sudo snap install doctl
# 授予额外权限
sudo snap connect doctl:kube-config
sudo snap connect doctl:ssh-keys :ssh-keys
sudo snap connect doctl:dot-docker
```

### 配置 doctl

首先打开 [Applications & API](https://cloud.digitalocean.com/account/api/tokens) 页面，创建一个令牌。

如果你使用 Ubuntu Snap 安装，则应该先创建配置目录：

```sh
mkdir ~/.config
```

登录：

```sh
doctl auth init
```

查看帐户信息以验证登录成功：

```sh
doctl account get
```

## 创建虚拟机

上传 SSH 公钥：

```sh
doctl compute ssh-key import <KEY-NAME> --public-key-file ~/.ssh/id_rsa.pub
```

在 SFO2 区域创建一个 Ubuntu 22.04 Droplet：

```sh
doctl compute droplet create \
    --region sfo2 \
    --image ubuntu-22-04-x64 \
    --size s-1vcpu-1gb \
    --ssh-keys <SSH-KEY-ID> \
    <DROPLET-NAME>
```

> 查看 size 列表：`doctl compute size list`

查看已经创建的虚拟机：

```sh
doctl compute droplet list
```

删除虚拟机：

```sh
doctl compute droplet delete <DROPLET-ID>
```

参考：

- [doctl CLI Command Reference](https://docs.digitalocean.com/reference/doctl/reference/)
- [How To Install and Configure doctl](https://docs.digitalocean.com/reference/doctl/how-to/install/)