## 上传公钥

如果远程计算机是类 Unix 系统，使用下面这条命令：

```powershell
Get-Content $Env:USERPROFILE\.ssh\id_rsa.pub | ssh USER@HOST "cat >> .ssh/authorized_keys"
```

## Troubleshooting

### Bad Permissions

```sh
$ ssh USER@HOST
Bad permissions. Try removing permissions for user: Domain\\User2 (S-1-5-21-479362186-2444553748-1039381088-1003) on file C:/Users/User/.ssh/id_rsa.
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions for 'C:\\Users\\User/.ssh/id_rsa' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "C:\\Users\\User/.ssh/id_rsa": bad permissions
```

解决方法：[windows 10, 自带的 OpenSSH, key 权限问题, 文件权限问题 | CSDN](https://blog.csdn.net/engineer520/article/details/82714696)