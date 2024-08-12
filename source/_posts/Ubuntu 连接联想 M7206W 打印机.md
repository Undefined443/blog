联想很多打印机其实是 [Brother](https://zh.wikipedia.org/zh-cn/%E5%85%84%E5%BC%9F%E5%B7%A5%E6%A5%AD) 打印机贴牌（[OEM](https://zh.wikipedia.org/zh-cn/%E4%BB%A3%E5%B7%A5%E7%94%9F%E4%BA%A7)）：

![image](https://img2024.cnblogs.com/blog/2778973/202408/2778973-20240804020018076-11566087.png)

所以你实际上在驱动列表中选一个 Brother 的驱动可能也能打印。

1. 将打印机连接到 Wi-Fi：

   在打印机上，按下 `功能` 按钮进入设置，通过上下键调整选项进入 `4.网络` > `1.无线网络` > `3.安装向导`，此时打印机会搜索 Wi-Fi 信号，搜索完成后选择你的 Wi-Fi 名称进入密码输入页，通过上下键调整字母，确认好之后按 `确认` 键输入下一个字母。当所有密码输入完成后，再次按下 `确认` 键，成功连接网络。

2. 在 Ubuntu 中打开 `设置` > `打印机`，选择添加打印机，选中 M7206W，在打印机设置中选择打印机详情，选择 `从数据库选择……`，然后选中 `Brother` 的 `DCP 7020 (recommended)` 驱动。

![image](https://img2024.cnblogs.com/blog/2778973/202408/2778973-20240804034259239-1118447850.png)

![image](https://img2024.cnblogs.com/blog/2778973/202408/2778973-20240804034326877-1705393245.png)

现在就可以使用打印机了。

附：

打印机的 Web 设置页在 `http://<your-printer-ip>:631`：

![image](https://img2024.cnblogs.com/blog/2778973/202408/2778973-20240804024950491-1507646605.png)

> 将 `<your-printer-ip>` 改为打印机实际 IP 地址

参见：

- [联想官网驱动下载](https://newsupport.lenovo.com.cn/driveDownloads_index.html)
- [总结各大常见打印机品牌在 Linux 下的驱动方法 | 云网牛站](https://www.ywnz.com/linuxjc/3341.html)
- [CUPS](https://zh.wikipedia.org/zh-cn/CUPS)
- [LPR](https://zh.wikipedia.org/wiki/%E8%A1%8C%E5%BC%8F%E6%89%93%E5%8D%B0%E6%9C%BA%E5%90%8E%E5%8F%B0%E7%A8%8B%E5%BA%8F%E5%8D%8F%E8%AE%AE)
- [Installing the LPR driver and CUPS wrapper driver (Linux®) | Brother Support](https://support.brother.com/g/b/faqend.aspx?c=us&lang=en&prod=lpql700eus&faqid=faqp00100414_000)