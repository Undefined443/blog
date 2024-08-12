> Microsoft 官方提供的 Windows PE 默认只有命令行界面。如果想要使用带有桌面环境的 Windows PE，推荐使用[微 PE](https://www.wepe.com.cn/download.html) 。

1. [下载并安装 Windows ADK 和 WinPE 加载项](https://learn.microsoft.com/zh-cn/windows-hardware/get-started/adk-install)

   安装 Windows ADK：

   ![image](https://img2024.cnblogs.com/blog/2778973/202408/2778973-20240812233054304-828966529.png)

   ![image](https://img2024.cnblogs.com/blog/2778973/202408/2778973-20240812233058996-1666070138.png)

   安装 WinPE 加载项：

   ![image](https://img2024.cnblogs.com/blog/2778973/202408/2778973-20240812233101408-1845165215.png)

2. 打开命令提示符（管理员）

   ```batch
   cd "C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Deployment Tools"
   .\DandISetEnv.bat
   cd "..\Windows Preinstallation Environment\amd64"
   md C:\WinPE_amd64\mount
   Dism /Mount-Image /ImageFile:"en-us\winpe.wim" /index:1 /MountDir:"C:\WinPE_amd64\mount"
   ```

   ```batch
   rmdir /S /Q C:\WinPE_amd64
   copype amd64 C:\WinPE_amd64
   ```

   制作启动盘。其中 P 是你的 U 盘的盘符

   ```batch
   MakeWinPEMedia /UFD C:\WinPE_amd64 P:
   ```

参考：

- [Windows PE (WinPE) | Microsoft Learn](https://learn.microsoft.com/zh-cn/windows-hardware/manufacture/desktop/winpe-intro?view=windows-11)

- [创建可启动 Windows PE 介质 | Microsoft Learn](https://learn.microsoft.com/zh-cn/windows-hardware/manufacture/desktop/winpe-create-usb-bootable-drive?view=windows-11)