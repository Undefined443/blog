产生错误的原因是软件没有签名。使用下面的命令给软件签名就好了。

```sh
sudo xattr -rd com.apple.quarantine /Applications/xxx.app
```