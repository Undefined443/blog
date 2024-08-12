### 安装 Samba 包

```sh
sudo apt install samba samba-common
```

### 创建用于 SMB 共享的文件夹

```sh
sudo mkdir /usr/local/volumes  # 新建用于共享的文件夹
sudo chown nobody:nogroup /usr/local/volumes  # 修改所属用户
sudo chmod 777 /usr/local/volumes  # 修改访问权限
```

### 添加 SMB 用户

添加你将要用来访问 SMB 服务的用户，必须使用一个系统中已经存在的用户。

```sh
sudo smbpasswd -a USER
```

> 将 `USER` 替换为系统中已经存在的用户

之后你登陆 SMB 服务器时，使用的用户名和密码就是刚刚设置的用户名和密码。

### 编辑 SMB 配置

```sh
sudo vim /etc/samba/smb.conf
```

在文件尾部添加如下内容

```ini
# Volumes 是你要在 SMB 服务器中显示的共享文件夹的名字
[Volumes]
  comment = TimeCapsule Volumes
  path = /usr/local/volumes
  browseable = yes
  writable = yes
  available = yes
  valid users = alan
```

### 重启 SMB 服务

重启 SMB 服务以应用修改

```sh
sudo service smbd restart
```

### 连接 SMB 服务器

打开访达，`⌘` `K` 打开连接服务器，输入 `smb://xx.xx.xx.xx` 连接。用户名和密码是之前添加到 SMB 服务的用户名和密码。

![image](https://img2024.cnblogs.com/blog/2778973/202404/2778973-20240421175231352-1541656575.png)

参考：[Ubuntu 20.04 搭建 Samba 服务](https://www.jianshu.com/p/ca5603a0f06b)