1. 编辑 `logind.conf` 文件

   你可以通过编辑 `/etc/systemd/logind.conf` 文件来控制盖子关闭时的行为：

   找到以下几行（如果不存在，可以手动添加）：

   ```ini
   #HandleLidSwitch=suspend
   #HandleLidSwitchExternalPower=suspend
   #HandleLidSwitchDocked=ignore
   ```

   将它们修改为如下：

   ```ini
   HandleLidSwitch=ignore
   HandleLidSwitchExternalPower=ignore
   HandleLidSwitchDocked=ignore
   ```

   这将告诉系统在盖子关闭时忽略这个事件，而不是进入睡眠或其他模式。

2. 重启 `systemd-logind` 服务以应用更改：

   ```sh
   sudo systemctl restart systemd-logind
   ```

存在的问题：现在合上盖子不仅不睡眠，连显示屏也不关了。还好我的电脑是一台破电脑，坏了也不心疼。如果电脑显示器是 OLED 的要注意烧瓶问题。

参考：

- [如何在笔记本电脑合盖时不挂起 Ubuntu | Linux 中国](https://linux.cn/article-15015-1.html)
- [【Ubuntu 20.04 LTS】设置笔记本合并盖子不休眠 | CSDN](https://blog.csdn.net/qq_31635851/article/details/124627990)