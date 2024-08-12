本人有强迫症，希望自己电脑上安装的软件永远是最新的。App Store 有自动更新功能，然而 Homebrew 则没有自动更新的选项。每次手动更新的话时间长了又感觉麻烦。后来发现可以使用 crontab 工具定时执行任务，于是想到可以利用 crontab 来实现 Homebrew 每日自动更新。

首先在一个合适的位置创建更新脚本：

```sh
vim upgrade.sh
```

在脚本中填入以下内容（注意将 brew 的路径换成你实际的路径）：

```sh
/opt/homebrew/bin/brew upgrade
```

然后编辑 `crontab`：

```sh
crontab -e
```

加入下面一行（注意将脚本的路径换成你实际的路径）：

```
0 23 * * * /path/to/upgrade.sh
```

这将会在每天 23:00 自动运行 `brew upgrade` 命令。

此外，`cron` 任务执行完成之后如果有输出的话会通过 Unix 邮件系统发送邮件（不是平常用的那种邮件）。你可以通过 `mail` 命令查看邮件。