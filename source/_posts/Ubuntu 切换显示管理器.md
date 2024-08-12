比较流行的显示管理器有：

- `gdm3` - GNOME Display Manager
- `lightdm` - Light Display Manager
- `sddm` - Simple Desktop Display Manager

查看当前使用的是哪个显示管理器：

```sh
ls -l /etc/systemd/system/display-manager.service
```

或者

```sh
cat /etc/X11/default-display-manager
```

切换显示管理器：

```sh
sudo dpkg-reconfigure lightdm
```

> 其实就是重新配置 `lightdm` 的安装包。当然你重新配置其他显示管理器的安装包也可以。

编辑显示管理器的默认桌面环境（前提是你使用 lightdm 显示管理器）：

```sh
sudo vim /etc/lightdm/lightdm.conf
```
