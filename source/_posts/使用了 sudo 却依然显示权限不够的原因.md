```sh
$ sudo echo "151.101.76.133 raw.githubusercontent.com" >> /etc/hosts
bash: /etc/hosts: 权限不够
```

bash 报错说权限不够，是因为重定向符号 `>` 和 `>>` 也是 bash 的命令。我们使用 `sudo` 只是让 `echo` 命令具有了 root 权限，但是没有让 `>` 和 `>>` 命令也具有 root 权限，所以 bash 会认为这两个命令都没有写入 hosts 文件的权限。

解决方法：

```sh
sudo sh -c "echo '151.101.76.133 raw.githubusercontent.com' >> /etc/hosts"
```

利用 `sh -c` 命令，它可以让 bash 将一个字符串作为完整的命令来执行，这样就可以将 `sudo` 的影响范围扩展到整条命令。
