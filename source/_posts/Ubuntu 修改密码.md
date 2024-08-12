## 强制修改密码

可以通过切换到 root 帐户来强制为普通用户修改密码：

```sh
sudo su      # 切换到 root 帐户
passwd USER  # 修改密码
```

## 设置空密码

Linux 每次安装软件都要输入密码，对于个人使用来说这点很烦，因此我们可以在自用电脑上设置空密码。

1. 为帐户启用 `NOPASSWD` 选项

   ```sh
   sudo visudo
   ```

   在文件底部添加下面一行：

   ```sh
   john ALL=(ALL) NOPASSWD:ALL
   ```

   > 将 `john` 改为你自己的用户名

2. 删除帐户密码

   ```sh
   sudo passwd -d $(whoami)
   ```

参考：

- [Can I set my user account to have no password? | Ask Ubuntu](https://askubuntu.com/questions/281074/can-i-set-my-user-account-to-have-no-password)
- [Disable authentication prompts in 15.04? | Ask Ubuntu](https://askubuntu.com/questions/614534/disable-authentication-prompts-in-15-04/614537#614537)

## Troubleshooting

如果你在打开一些应用时遇到授权提示：

![image](https://img2024.cnblogs.com/blog/2778973/202408/2778973-20240804011240458-195120320.png)

这是因为你启用了 Automatic Login 导致的。在 `设置` > `系统` > `用户` 中关闭 Automatic Login：

![image](https://img2024.cnblogs.com/blog/2778973/202408/2778973-20240804014106420-2066424069.png)
