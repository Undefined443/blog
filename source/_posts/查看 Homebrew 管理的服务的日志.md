## TL;DR

首先找到 log 文件的位置：

- 对于 macOS (arm64)，log 文件在 `/opt/homebrew/var/log` 目录下
- 对于 macOS (x86_64)，log 文件在 `/usr/local/homebrew/var/log/` 目录下
- 对于 Linux，log 文件在 `/home/linuxbrew/.linuxbrew/var/log` 目录下

查看 log 文件：

```sh
$ tail -fn 10 <file>
```

> 将 `<file>` 改为你要查看的 log 文件

## macOS

首先启动服务：

```sh
$ brew services start frpc
==> Successfully started `frpc` (label: homebrew.mxcl.frpc)
```

可以看到这里 Homebrew 在注册 frpc 服务时给了我们一个 `label`，这个 `label` 是服务的标识符。

Homebrwe 注册服务时会在启动目录写入相应的配置文件，这个配置文件的名字就是刚刚 Homebrew 给我们的 `label`。

根据我们启动服务时使用的命令的不同，启动目录的位置也会有所不同。

- 如果你使用不带 `sudo` 的启动命令，那么配置文件会写入到用户登录自启的启动目录 `~/Library/LaunchAgents`
- 如果你使用带 `sudo` 的启动命令，那么配置文件会写入到开机自启的启动目录 `/Library/LaunchDaemons`

刚刚的我们使用的是不带 `sudo` 的启动命令，因此我们到 `~/Library/LaunchAgents` 目录下寻找我们的配置文件：

```sh
$ ls ~/Library/LaunchAgents
homebrew.mxcl.frpc.plist
```

可以看到这里有一个名为 `homebrew.mxcl.frpc.plist` 的配置文件，它的文件名前缀刚好就是之前 Homebrew 给我们的 `label`。

我们打开这个配置文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>KeepAlive</key>
	<true/>
	<key>Label</key>
	<string>homebrew.mxcl.frpc</string>
	<key>LimitLoadToSessionType</key>
	<array>
		<string>Aqua</string>
		<string>Background</string>
		<string>LoginWindow</string>
		<string>StandardIO</string>
		<string>System</string>
	</array>
	<key>ProgramArguments</key>
	<array>
		<string>/opt/homebrew/opt/frpc/bin/frpc</string>
		<string>-c</string>
		<string>/opt/homebrew/etc/frp/frpc.toml</string>
	</array>
	<key>RunAtLoad</key>
	<true/>
	<key>StandardErrorPath</key>
	<string>/opt/homebrew/var/log/frpc.log</string>
	<key>StandardOutPath</key>
	<string>/opt/homebrew/var/log/frpc.log</string>
</dict>
</plist>
```

可以看到在文件的底部有两个配置项 `StandardErrorPath` 和 `StandardOutPath`，它们的值 `/opt/homebrew/var/log/frpc.log` 就是我们要找的 log 文件的位置。

我们可以通过下面的命令来查看 log 文件：

```sh
$ tail -fn 10 /opt/homebrew/var/log/frpc.log
```

这将跟踪显示 log 文件中的最后 10 条内容。

## Linux

首先，使用 Homebrew 启动服务时你会看到一条类似如下的信息：

```sh
$ brew services start frps
Created symlink /home/user/.config/systemd/user/default.target.wants/homebrew.frps.service → /home/user/.config/systemd/user/homebrew.frps.service.
==> Successfully started `frps` (label: homebrew.frps)
```

这表示 Homebrew 在 `/home/user/.config/systemd/user/default.target.wants` 目录下为一个名为 `homebrew.frps.service` 的服务单元文件创建了一个符号链接，使其能够在 `default.target` 达成时启动（即用户登录时）。

我们查看服务单元文件 `homebrew.frps.service` 的内容：

```ini
[Unit]
Description=Homebrew generated unit for frps

[Install]
WantedBy=default.target

[Service]
Type=simple
ExecStart=/home/linuxbrew/.linuxbrew/opt/frps/bin/frps -c /home/linuxbrew/.linuxbrew/etc/frp/frps.toml
Restart=always
StandardOutput=append:/home/linuxbrew/.linuxbrew/var/log/frps.log
StandardError=append:/home/linuxbrew/.linuxbrew/var/log/frps.log
```

可以看到在文件的底部有两个配置项 `StandardOutput` 和 `StandardError`，它们的值 `/home/linuxbrew/.linuxbrew/var/log/frps.log` 就是我们要找的 log 文件的位置。

我们可以通过下面的命令来查看 log 文件：

```sh
$ tail -fn 10 /home/linuxbrew/.linuxbrew/var/log/frps.log
```

这将跟踪显示 log 文件中的最后 10 条内容。