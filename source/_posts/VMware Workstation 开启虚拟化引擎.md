> 摘要：想开启 VMware Workstation 虚拟机上的虚拟化引擎相关设置，却发现打不开。在网上一番搜集之后找到了解决办法。
>
> ⚠️ 注意：进行下面的设置后将导致无法启动虚拟机的 Docker。支持虚拟化引擎和支持 Docker 只能二选一。

1. 关闭 Windows 功能

    打开 `启用或关闭 Windows 功能`，并关闭以下功能：

    - Hyper-V
    - Windows 沙盒
    - Windows 虚拟机监控程序平台
    - 容器
    - 适用于 Linux 的 Windows 子系统
    - 虚拟机平台

    ![image](https://s2.loli.net/2024/07/07/qBaeISpXb6HhYAv.png =400x)


2. 关闭内存完整性

    打开 `Windows 安全中心` > `设备安全性` > `内核隔离` > `内核隔离详细信息`，将 `内存完整性` 选项关闭。

![image](https://s2.loli.net/2024/07/07/jXDKA2tBqOPaCTz.png)

重启之后，再打开 VMware Workstation 的虚拟化引擎相关设置，你就发现能正常启动虚拟机了。