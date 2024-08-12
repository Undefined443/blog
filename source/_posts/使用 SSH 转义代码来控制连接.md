OpenSSH 最常被忽视的一个非常有用的功能是能够从连接内部控制会话的某些方面。通过使用 SSH 转义代码，我们能够在会话内部与本地 SSH 软件进行交互。

### 强制从客户端断开连接（如何退出卡住或冻结的会话）

这些命令可以在 SSH 会话中以 `~` 控制字符开头执行。**只有在换行后第一次键入时才会解释控制命令，因此在使用之前始终按 ENTER 键一两次。**

我们可以从客户端发起断开连接。SSH 连接通常由服务器关闭，但如果服务器遇到问题或连接已中断，则可能会出现问题。通过使用客户端断开，可以从客户端清洁地关闭连接。

要从客户端关闭连接，请使用控制字符 `~`，后跟一个句点 `.`。如果您的连接出现问题，您可能会发现自己处于似乎被卡住的终端会话中。尽管缺乏反馈，仍需输入命令以执行客户端断开连接：

```sh
[ENTER]
~.
```

连接应立即关闭，将您带回本地 shell 会话。

### 将一个 SSH 会话放入后台

按下控制字符 `~`，然后执行将任务放到后台的常规键盘快捷键 `Ctrl` `Z`：

```sh
[ENTER]
~[CTRL-z]
```

这将把连接放到后台，将您返回到本地 shell 会话。要返回到 SSH 会话，您可以使用传统的作业控制机制。

您可以通过输入 `fg` 来立即重新激活您最近的后台任务：

```sh
fg
```

如果您有多个后台任务，您可以通过输入以下命令来查看可用的作业：

```sh
$ jobs
[1]+  Stopped                 ssh username@some_host
[2]   Stopped                 ssh username@another_host
```

您可以通过使用第一列中的索引和百分号将任何任务带到前台：

```sh
fg %2
```

### 更改现有 SSH 连接上的端口转发选项

用户可以在连接已经建立之后更改端口转发配置。这允许您即时创建或拆除端口转发规则。

这些功能是 SSH 命令行界面的一部分，可以在会话期间通过使用控制字符 `~` 和 `C` 来访问：

```sh
[ENTER]
~C
ssh>
```

您将获得一个 SSH 命令提示符，其中只有非常有限的一组有效命令。要查看可用选项，您可以从此提示符中键入 `-h`。如果没有返回任何内容，您可能需要使用 `~v` 几次来增加 SSH 输出的详细程度：

```sh
[ENTER]
~v
~v
~v
~C
-h
Commands:
      -L[bind_address:]port:host:hostport    Request local forward
      -R[bind_address:]port:host:hostport    Request remote forward
      -D[bind_address:]port                  Request dynamic forward
      -KL[bind_address:]port                 Cancel local forward
      -KR[bind_address:]port                 Cancel remote forward
      -KD[bind_address:]port                 Cancel dynamic forward
```

正如您所看到的，您可以轻松地使用适当的选项来实现任何转发选项（有关更多信息，请参阅转发部分）。您还可以使用与转发类型字母前的 `K` 指定的 `kill` 命令来销毁隧道。例如，要销毁本地转发 `-L`，您可以使用 `-KL` 命令。您只需要为此提供端口。

因此，要设置本地端口转发，您可以输入：

```sh
[ENTER]
~C
-L 8888:127.0.0.1:80
```

您本地计算机上的端口 8888 现在将能够与您正在连接的主机上的 Web 服务器进行通信。完成后，您可以通过输入以下内容来关闭该转发：

```sh
[ENTER]
~C
-KL 8888
```

参考：[使用 SSH 转义代码来控制连接](https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys#using-ssh-escape-codes-to-control-connections)