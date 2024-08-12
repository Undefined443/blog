## Ubuntu

```sh
sudo apt install texlive-full
```

其他可用软件包：

| 软件包                    | 压缩包  | 磁盘空间 |
| ------------------------- | :-----: | :------: |
| texlive-latex-base        |  59 MB  |  216 MB  |
| texlive-latex-recommended |  74 MB  |  248 MB  |
| texlive-pictures          |  83 MB  |  277 MB  |
| texlive-fonts-recommended |  83 MB  |  281 MB  |
| texlive                   |  98 MB  |  314 MB  |
| texlive-plain-generic     |  82 MB  |  261 MB  |
| texlive-latex-extra       | 144 MB  |  452 MB  |
| texlive-full              | 2804 MB | 5358 MB  |

- 关于各个软件包区别的讨论：[Differences between texlive packages in Linux | Stack Exchange](https://tex.stackexchange.com/questions/245982/differences-between-texlive-packages-in-linux)

- 关于 TeX Live 在所有类 Unix 系统上的通用安装方法，请参见：[TeX Live - Quick install for Unix | tug.org](https://www.tug.org/texlive/quickinstall.html)

## macOS

参见 [MacTeX 使用 | 博客园](https://www.cnblogs.com/Undefined443/p/-/mactex)。

## Windows

### 使用网络安装程序

下载并运行安装程序：[install-tl-windows.exe](https://mirror.ctan.org/systems/texlive/tlnet/install-tl-windows.exe)

### 使用镜像源

1. 下载 TeX Live 的 ISO 镜像：[texlive.iso | 中科大镜像源](https://mirrors.ustc.edu.cn/CTAN/systems/texlive/Images/texlive.iso)

2. 挂载镜像，并运行其中的安装脚本 `install-tl-windows.bat`。

参考：[TeX Live on Windows | tug.org](https://www.tug.org/texlive/windows.html)