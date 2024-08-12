SSH 在连接远程机器时默认会传递一些环境变量，其中就包括你本机的 locale 变量。这会导致远程机器的 locale 配置变成和你本地主机一样。有时候我们不希望这种行为，我们可以通过修改 SSH 配置文件来取消这一行为。

编辑 `/etc/ssh/ssh_config` 文件：

```sh
sudo vim /etc/ssh/ssh_config
```

可以找到文件中有一项 `SendEnv LANG LC_*` 设置：

```
Host *
    SendEnv LANG LC_*
```

这个设置导致了我们在使用 SSH 连接时自动传递了 `LANG` 和 `LC_*` 环境变量。把这条设置注释掉就可以了：

```
#Host *
#    SendEnv LANG LC_*
```

StackOverflow 中提到还可以在 `~/.ssh/config` 中设置取消传递 `LANG` 和 `LC_*` 环境变量的方法，但是 SSH 貌似会默认采用系统设置中的环境变量配置，因此这个方法无效。

参考：[How not to pass the locale through an ssh connection command | StackOverflow](https://stackoverflow.com/questions/29609371/how-not-to-pass-the-locale-through-an-ssh-connection-command)