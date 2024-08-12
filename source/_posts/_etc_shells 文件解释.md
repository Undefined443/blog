`/etc/shells` 文件是 UNIX 和类 UNIX 操作系统中的一个文本文件，它列出了系统上认为是合法的、用户可以选择的 shell 的完整路径。这个文件对于系统安全和用户环境配置很重要。

以下是 `/etc/shells` 文件的几个用途：

1. 验证用户登录： 当用户尝试通过程序如 `chsh`（更改用户登录 shell 的命令）更改其登录 shell 时，程序会检查 `/etc/shells` 文件来确认用户选择的 shell 是否被系统列为合法的 shell。
2. FTP 访问控制： 一些 FTP 服务器（如 vsftpd）会检查 `/etc/shells` 文件以确定用户是否有权限使用 FTP 服务。通常只有那些拥有列出在 `/etc/shells` 文件中的 shell 的用户才被允许访问 FTP 服务。
3. Mail Transfer Agents（MTAs）： 某些 Mail Transfer Agents 程序会检查 `/etc/shells` 文件来确定一个用户是否拥有一个有效的 shell，基于这一信息来决定用户是否能够收发邮件。
4. 限制 cron 作业： 某些系统用 `/etc/shells` 来确定一个用户是否可以安排 cron 作业。

`/etc/shells` 文件的格式非常简单，由换行符分隔的 shell 路径列表组成。下面是一个典型的 `/etc/shells` 文件的示例：

```plaintext
/bin/sh
/bin/bash
/usr/bin/bash
/bin/tcsh
/bin/csh
/bin/ksh
/usr/bin/ksh
/bin/zsh
/usr/bin/zsh
```

在上面的例子中，列出的每个路径都代表一个被系统认为合法的 shell。这意味着用户可以选择这些 shell 作为他们的默认登录 shell，也表明了 FTP 和邮件服务可能依赖此文件来提供服务。

管理员可以通过编辑 `/etc/shells` 文件来添加或是删除合法 shell，以符合安全政策或是用户需求。