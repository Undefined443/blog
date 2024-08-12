## 连接 Xfce 4 远程桌面

1. 下载 Xfce 4 桌面环境：

   ```sh
   sudo apt install -y xfce4 xfce4-goodies
   ```

   这里会提示你设置显示管理器，我们设置 `gdm3` 就好。

2. 安装 TightVNC 服务器

   ```sh
   sudo apt install -y tightvncserver
   ```

3. 接下来，运行 `vncserver` 命令设置 VNC 访问密码，创建初始配置文件，并启动 VNC 服务器实例：

   ```sh
   vncserver
   ```

   系统将提示你设置密码，这里需要填入一个长度小于 8 位的密码，多余的位数会被截去。之后你要再输入一遍相同的密码以确认密码。接下来系统会提示你是否要设置提供仅查看功能的密码，一般我们不需要这个功能，所以这里填 `n`。

   启动 VNC 服务器时注意命令的输出提示。如果提示像下面这样，表示 1 号 VNC 服务器实例已被占用，创建了 2 号 VNC 服务器实例：

   ```
   Warning: inspiron-3468:1 is taken because of /tmp/.X1-lock
   Remove this file if there is no X server inspiron-3468:1

   New 'X' desktop is inspiron-3468:2

   Starting applications specified in /home/xiao/.vnc/xstartup
   Log file is /home/xiao/.vnc/inspiron-3468:2.log
   ```

4. 此时我们已经设置好了密码。关闭 VNC 服务器实例：

   ```sh
   vncserver -kill :2  # 刚刚创建了几号实例就关闭几号实例
   ```

5. 接下来我们要修改 `xstartup` 文件。`~/.vnc/xstartup` 文件用于配置通过 VNC 连接启动的远程桌面环境或窗口管理器，在 VNC 服务器启动会话时被执行。

   ```sh
   mv ~/.vnc/xstartup{,.bak}  # 先备份原文件
   vim ~/.vnc/xstartup        # 创建新文件
   ```

   填入以下配置：

   ```sh
   #!/bin/bash
   [ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources  # 加载用户的 .Xresources 配置文件到 X 资源数据库中
   startxfce4 &  # 启动 XFCE 桌面环境（后台执行）
   ```

6. 为文件添加执行权限：

   ```sh
   chmod 755 ~/.vnc/xstartup
   ```

7. 启动 VNC 服务器实例：

   ```sh
   vncserver -localhost
   ```

   由于 VNC 不具有加密功能，因此在公网上直接和 VNC 服务器通信并不安全。我们接下来将建立一个 SSH 隧道，通过隧道我们和 VNC 服务器连接。这种策略将为 VNC 增加一层额外的安全性，因为唯一能够访问它的用户是那些已经具有 SSH 访问权限的用户。

8. 建立 SSH 隧道

   ```sh
   ssh USER@HOST -L 5901:localhost:5901 -CNf  # 注意改端口号，端口号应为 5900 + 实例号
   ```

   - `-L 59000:localhost:5901`: -L 选项指定本地计算机端口 `5901` 被转发到目标服务器的 `localhost:5901`。
   - `-C`：此标志启用压缩，有助于减少资源消耗并加快速度。
   - `-N`: 此选项告诉 SSH 你不希望执行任何远程命令。当您只想转发端口时，此设置很有用。
   - `-f`: 此选项表示将 SSH 连接放到后台。避免长时间不用 SSH 连接时终端失去响应。

9. 使用 VNC 客户端进行连接

   如果你使用 macOS，可以使用系统自带的屏幕共享应用来连接：打开 Spotlight，搜索并打开 `Sharing.app`。之后新建一个连接，连接地址填写 `vnc://localhost:5901`（注意改端口号为实际端口号）

   ![image](https://s2.loli.net/2024/07/07/TsEY3WxBVgyhSc2.png)

   如果一切正常，你就能看到 Xfce 的远程桌面了。

   ![image](https://s2.loli.net/2024/08/03/3JzDVfdFSoALxBs.png)

## 设置开机自启

1. 创建 systemd 服务文件：

   ```sh
   sudo vim /etc/systemd/system/vncserver@.service
   ```

   > 在名称末尾的 `@` 符号将允许我们传入一个参数，你可以在服务配置中使用。你将使用此参数来指定在管理服务时要使用的 VNC 显示端口。

   将以下行添加到文件中。确保更改 `User`、`Group`、`WorkingDirectory` 的值，并将 `PIDFILE` 值中的用户名更改你的用户名：

   ```ini
   [Unit]
   Description=Start TightVNC server at startup
   After=syslog.target network.target

   [Service]
   Type=forking
   User=<USER>
   Group=<GROUP>
   WorkingDirectory=/home/<USER>

   PIDFile=/home/<USER>/.vnc/%H:%i.pid
   ExecStartPre=-/usr/bin/vncserver -kill :%i > /dev/null 2>&1
   ExecStart=/usr/bin/vncserver -depth 24 -geometry 1280x800 -localhost :%i
   ExecStop=/usr/bin/vncserver -kill :%i

   [Install]
   WantedBy=multi-user.target
   ```

2. 重启 systemd 守护进程

   ```sh
   sudo systemctl daemon-reload
   ```

3. 启用服务文件

   ```sh
   sudo systemctl enable vncserver@1.service
   ```

   > 这里的 `1` 表示服务应该出现在哪个显示编号上，这里为 `:1`。

   现在，每次启动系统时，都会自动启动一个编号为 `:1` 的 VNC 服务器实例。

4. 建立 SSH 隧道

   在每次连接 VNC 服务之前，你都需要确保已经建立了 SSH 隧道：

   ```sh
   ssh USER@HOST -L 59000:localhost:5901 -CNf
   ```

## Troubleshooting

### 灰屏问题

![image](https://s2.loli.net/2024/08/03/EId7KGr6jNaCWtu.png)

1. 检查 `~/.vnc/xstartup` 配置文件的内容是否正确，以及文件是否有执行权限。
2. 使用 `ps -a` 命令检查 `gnome-session-b` 进程是否在运行：

   ```sh
   $ ps -a | grep gnome-session-b
      9521 tty2     00:00:00 gnome-session-b
   ```

   发现 `gnome-session-b` 进程在运行，此时无法启动 Xfce 桌面环境。注意我们必须通过 SSH 远程连接到服务器上进行接下来的操作，不能在主机的桌面环境下操作。

   关闭 gnome-session-b：

   ```sh
   kill $(ps -a | grep gnome-session-b | awk '{print $1}')
   ```

   然后重启 VNC 服务器：

   ```sh
   vncserver -kill :2  # 记得替换为你实际的实例号
   vncserver
   ```

   此时再次尝试连接，应该就能看到桌面了。

参考：[How to Install and Configure VNC on Ubuntu 22.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-vnc-on-ubuntu-22-04)

参见：[Ubuntu 设置远程桌面（RDP）](https://www.cnblogs.com/Undefined443/p/18137304)

个人认为 RDP 更好用一点