```sh
cd "/Applications/Microsoft Edge.app/Contents/Frameworks/Microsoft Edge Framework.framework/Versions"
ls # 查看已安装的 Edge 版本
rm -rf <old_version>

cd ~/"Library/Application Support/Microsoft/EdgeUpdater/apps/msedge-stable"
ls # 查看已安装的 Edge 版本
rm -rf <old_version>

sudo shutdown -r now
```

> 如何查找软件位置： > 系统信息(⌥) > 应用程序(系统设置 > 通用 > 关于本机 > 系统报告 > 应用程序)