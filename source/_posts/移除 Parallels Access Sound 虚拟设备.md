在安装了 Parallels 之后，发现 Mac 没声音，打开声音设置一看音频输出设备被设为了 Parallels Access Sound。把输出设备调回 MacBook 扬声器就有声音了。

但是音频输出设备经常被自动切换回 Parallels Access Sound。于是我决定移除这个设备。

打开 `/Library/Audio/Plug-Ins/HAL` 文件夹，你会找到一个名为 `com.parallels.mobile.audio.driver` 的文件夹。删除这个文件夹，重启，你就会发现 Parallels Access Sound 不见了。

```sh
cd /Library/Audio/Plug-Ins/HAL
rm -rf com.parallels.mobile.audio.driver
```

参考：[How To Remove Parallels Access Sound 1 & 2 Audio Devices](https://forum.parallels.com/threads/how-to-remove-parallels-access-sound-1-2-audio-devices.353346/)