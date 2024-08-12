### 设置 SMB 服务

首先在 Ubuntu 中配置 SMB 服务。可以参考 [Ubuntu 设置 SMB 服务](https://www.cnblogs.com/Undefined443/p/18149166)。

### 创建 APFS 磁盘映像

我们在 Ubuntu 上创建出的 SMB 共享文件夹可以用来存放文件，但是不能直接用来存放时间机器备份。因为时间机器是基于 APFS 文件系统的，而我们的 Linux 使用的是 Ext4 文件系统。

解决方法是我们可以在 SMB 共享文件夹里放一个 APFS 磁盘映像，然后挂载这个磁盘映像，再将这个磁盘映像作为我们的时间机器备份盘。

打开“磁盘工具”，`⌘` `N` 创建新的空白磁盘映像。磁盘映像的存储位置我们可以直接选择刚刚创建的 SMB 共享文件夹。映像格式选择稀疏捆绑磁盘映像。

> 稀疏磁盘映像的特点是其实际大小是随着你的使用逐渐扩大的，不像“读/写磁盘映像”一开始设置了大小是多少就是多大。稀疏捆绑磁盘映像的大小一开始很小，只有在需要更多空间时才进行扩展。其设置的大小只是对最大大小的一个限制。关于稀疏映像的介绍可以看维基百科：[Sparse Image | Wikipedia](https://en.wikipedia.org/wiki/Sparse_image)
>
> 而稀疏捆绑磁盘映像则是把原先的一整个磁盘映像文件分为多个小文件，可以查看 [Disk images: Sparse vs. Sparse bundle? | Apple Community](https://discussions.apple.com/thread/2001162?sortBy=best)
>
> 对于SSD（固态硬盘）来说，稀疏捆绑磁盘映像通常是更好的选择，原因包括：
>
> - 写放大（Write Amplification）的减少：由于SSD的写操作相对有限，稀疏捆绑磁盘映像可以通过只修改变动的部分，而不是修改整个文件，来减少对SSD的写操作。
>
> - 效率和性能：稀疏捆绑格式允许单独对文件包中的部分文件进行操作和同步，这在频繁变更数据的情况下（如使用 Time Machine 备份）可以提高效率和性能。

![image](https://img2024.cnblogs.com/blog/2778973/202404/2778973-20240421184624359-1464890900.png)

创建磁盘映像时会要求填写两个名字，一个是“存储为”，另一个是“名称”。这里的“存储为”填写的名字是之后创建的映像文件的名字，比如 TimeMachine.sparsebundle。

![image](https://img2024.cnblogs.com/blog/2778973/202404/2778973-20240421184721718-1479619769.png)

“名称”填写的名字是挂载映像文件后磁盘映像的名字，比如双击 TimeMachine.sparsebundle 文件挂载映像后就会看到桌面出现了一个名为 Time Machine 的磁盘。

![image](https://img2024.cnblogs.com/blog/2778973/202404/2778973-20240421184716377-794640237.png)

创建磁盘映像文件后会自动挂载磁盘映像，我们先推出磁盘映像，然后将磁盘映像文件 TimeMachine.sparsebundle 移动到 SMB 共享文件夹，然后再双击 TimeMachine.sparsebundle 挂载映像。

### 设置时间机器备份盘

然后我们使用命令设置时间机器备份盘为我们挂载的磁盘映像：

```sh
sudo tmutil setdestination "/Volumes/Time Machine"
```

> 如果你的磁盘映像的名字不是 `Time Machine`，则需要将其改为你磁盘映像的名字。

#### 移除时间机器备份地址

如果你需要移除时间机器备份盘，需要首先查看当前时间机器备份盘的 ID：

```sh
$ tmutil destinationinfo
====================================================
Name          : Time Machine
Kind          : Local
Mount Point   : /Volumes/Time Machine
ID            : CD188373-789A-4AD5-B410-4A47A3B53FE8
```

可以看到名为“Time Machine”的备份盘的 ID 是 `CD188373-789A-4AD5-B410-4A47A3B53FE8`。拷贝这个 ID，然后运行：

```sh
sudo tmutil removedestination CD188373-789A-4AD5-B410-4A47A3B53FE8
```

### 使用体验

实际体验下来，发现基于 SMB 的时间机器备份很慢。我使用本地连接的 Samsung T7 SSD 进行备份时需要的时间大概为 2 个小时。而使用 SMB 文件共享在远程机器的西数蓝盘 SSD 上备份则需要 10 个小时左右。

![image](https://img2024.cnblogs.com/blog/2778973/202404/2778973-20240421220910564-702881792.png)

参考：

1. [苹果 macOS 时间机器备份至 windows linux 等 SMB 服务器](https://zhuanlan.zhihu.com/p/582229314)
2. [树莓派、Windows 设备都可以做你 Mac 的「时间机器」——利用 SMB 协议进行 Time Machine 备份 | 少数派](https://sspai.com/post/57539)