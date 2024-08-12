## 安装

macOS:

```sh
brew install apache2
```

Ubuntu:

```sh
sudo apt install apache2
```

## 使用

配置文件路径：

- macOS: `/opt/homebrew/etc/httpd/httpd.conf`
- Ubuntu: `/etc/apache2/apache2.conf`

DocumentRoot:

- macOS: `/opt/homebrew/var/www`
- Ubuntu: `/var/www`

### 启动

macOS:

```sh
brew services start httpd
```

Ubuntu:

```sh
sudo systemctl start apache2
```

### 操作

```sh
apachectl
```

参见：[Apache HTTP Server Documentation](https://httpd.apache.org/docs/current/)