### 安装 VSFTP 服务

```sh
sudo apt install vsftpd
```

### 检查配置文件

```sh
sudo vim /etc/vsftpd.conf
```

确保以下配置项正确：

```sh
#禁止匿名访问
anonymous_enable=NO
#接受本地用户
local_enable=YES
#允许上传
write_enable=YES
```

### 允许 VSFTP 服务开机自启

```sh
sudo systemctl enable vsftpd
```

### 连接 FTP 服务器

打开访达，`⌘` `K` 打开连接服务器，输入 `ftp://xx.xx.xx.xx`，点连接。

![image](https://img2024.cnblogs.com/blog/2778973/202404/2778973-20240421173614530-1385881412.png)

用户名和密码是你 FTP 服务器系统上已有的用户名和密码。

参考：[超简单 Ubuntu Server 配置 FTP 服务器教程](https://blog.csdn.net/weixin_36656836/article/details/127462864)