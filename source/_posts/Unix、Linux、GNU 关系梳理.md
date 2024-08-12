之前写了一篇 MSYS2、MinGW 和 Cygwin 关系梳理的博客，但是要讲清它们几个的关系最好还是先了解一下操作系统的发展历程。遂补充了这篇博客。

## UNIX：现代操作系统的始祖

[Operating Systems: Crash Course Computer Science | YouTube](https://youtu.be/26QPDBe-NB8?t=526&si=HUmKbvaeZf15jj8N)

现在我们常用的操作系统，除了 Windows 外，剩下的几乎所有操作系统，都可以追溯到 [UNIX](https://en.wikipedia.org/wiki/Unix)。

1964 年，麻省理工学院（MIT）、通用电气（GE）、贝尔实验室（Bell Labs）想要开发一款先进的操作系统，他们将该操作系统起名为 [Multics](https://zh.wikipedia.org/wiki/Multics)，其名称来源于 MULTiplexed Information and Computing System（多工信息与计算系统）。然而，Multics 系统的开发难度很高，项目的开发进度十分缓慢，因此后来贝尔实验室退出了这个项目。但是贝尔实验室的两名研究员 [Ken Thompson](https://en.wikipedia.org/wiki/Ken_Thompson) 和 [Dennis Ritchie](https://en.wikipedia.org/wiki/Dennis_Ritchie) 转而设计了另一款更简单的操作系统，UNIX。UNIX 系统引入了内核的概念，内核只实现操作系统的核心功能，而其他功能均由外部的工具程序实现。UNIX 最初由汇编语言编写，为了能够将 UNIX 移植到其他机器，Dennis Ritchie 又发明了 C 语言，并和 Ken Thompson 一起使用 C 语言重写了 UNIX。UNIX 的简单性使得它可以在更便宜和更多元的机器上运行，并很快在贝尔实验室内部流行了起来。随着 UNIX 的源码被传播到各大高校，UNIX 很快成为了 1970 -- 80 年代最受欢迎的操作系统之一。

拿到 UNIX 源码的高校纷纷开始尝试对 UNIX 进行扩展和改进，从而形成了自己版本的 UNIX，称为“UNIX 变种”。其中最著名的变种之一是来自加州大学伯克利分校（UCB）的学生 [Bili Joy](https://en.wikipedia.org/wiki/Bill_Joy) 开发的变种，称为 [BSD](https://zh.wikipedia.org/wiki/BSD)（Berkeley Software Distribution）。

然而好景不长，UNIX 的大受欢迎使得贝尔实验室的母公司 AT&T 看到了 UNIX 的商业价值，并决定将 UNIX 转为商业软件。于是在 1979 年，AT&T 决定不再将 UNIX 源码免费分发给高校，并发行了一个商业版的 UNIX，称为 [UNIX System V](https://zh.wikipedia.org/wiki/UNIX_System_V)，同时对之前的 UNIX 及其变种声明了著作权。

从事计算机教学但又无法获取 UNIX 源码的 [Andrew S. Tanenbaum](https://en.wikipedia.org/wiki/Andrew_S._Tanenbaum) 教授为了方便教学，决定自行编写一个类 UNIX 的操作系统内核，并将其命名为 [MINIX](https://zh.wikipedia.org/wiki/MINIX)。MINIX 与 UNIX 完全兼容，但是没有使用任何 UNIX 源码。

## GNU：自由软件运动

[Richard Stallman](https://en.wikipedia.org/wiki/Richard_Stallman) 是一个计算机大神，从小就爱写代码。不仅热爱写代码，他还热衷于分享代码。为了让更多人加入共享代码的行动，他于 1984 年创建了 [GNU 项目](https://en.wikipedia.org/wiki/GNU_Project)（GNU's Not Unix）。最初的设想是建立一个免费版本的 UNIX 系统，然而当时的 GNU 项目只有 Stallman 一个人，构建一个完整的操作系统是一项过于庞大的工作。于是 Stallman 想，他可以先开发操作系统的工具软件，之后再开发操作系统内核，最后将工具软件和内核组合起来，就能组成一个完整的操作系统。为了宣传 GNU，Stallman 开始参考 UNIX 上的专有软件，并开发了完全免费且公开源代码的 GNU 版本。后来 Stallman 还开发了著名的编辑器 Emacs，以及大名鼎鼎的 GCC 编译套件。与此同时他还成立了自由软件基金会（[Free Software Foundation](https://en.wikipedia.org/wiki/Free_Software_Foundation), FSF）。为了避免 GNU 项目所开发的软件被其他人修改后再次变成专有软件，Stallman 与律师草拟了通用公共许可证（[General Public License](https://en.wikipedia.org/wiki/GNU_General_Public_License), GPL），并称它为“[Copyleft](https://zh.wikipedia.org/wiki/Copyleft)”（相对于专有软件的“Copyright”）。在 GPL 条款下，任何使用了 GPL 源码的软件都必须再次以 GPL 条款发布，且公开源代码。由于 GNU 项目开发了很多重要而又基础的软件，且这些软件都遵守 GPL 协议，因此出现了很多基于这些软件且同样遵守 GPL 协议的软件，从而壮大了自由软件群体。不过，GNU 项目的目标是构建一个完整的操作系统。现在操作系统的工具软件有了，还差一个操作系统内核，就可以完成 GNU 项目了。GNU 项目提出了一个名为 [Hurd](https://en.wikipedia.org/wiki/GNU_Hurd) 的内核，然而这个内核的设计理念过于先进，实现难度过大，一直迟迟没能发布。

## Linux

1991 年，一个名为 [Linus Torvalds](https://en.wikipedia.org/wiki/Linus_Torvalds) 的芬兰小伙正在大学读计算机专业。他购买了一台 Intel 386 计算机，想要在上面运行 Unix 系统。然而当时的 Unix 系统是收费的，正好他了解到 MINIX 完全兼容 UNIX 且可以在 Intel 386 机器上运行，于是他就体验了一段时间 MINIX。然而毕竟 MINIX 仅用于教学目的，缺少很多功能，于是 Linus 想要改进 MINIX。他阅读了 MINIX 的源码，并使用 GNU 开源工具开发了一个简易的操作系统内核，Linus 将其命名为 [Linux](https://en.wikipedia.org/wiki/Linux)。为了改进 Linux，Linus 将其源码放到网上，并号召大家一起为这个操作系统提建议。由于很多人都对这种能运行在个人计算机上的类 UNIX 系统感兴趣，他的 Linux 系统很快获得了很多人的帮助。一开始，Linus 对源码的修改来自于他人通过邮件发来的源码，后来由于提交修改意见的人越来越多，Linus 开始使用版本控制软件 [BitKeeper](https://en.wikipedia.org/wiki/BitKeeper) 来帮助自己管理代码。然而后来 BitKeeper 的免费许可被取消，Linus 一气之下写了自己版本的版本控制软件 [Git](https://en.wikipedia.org/wiki/Git)，并开源免费发布。

由于 BSD 在 UNIX 历史中的巨大影响，UNIX 的版权持有者 USL 和 BSD 的开发者 UCBi 为 UNIX 的版权打了一场旷日持久的大官司（[USL v. BSDi](https://en.wikipedia.org/wiki/UNIX_System_Laboratories,_Inc._v._Berkeley_Software_Design,_Inc.)）。官司的结局是 BSD 不能再使用原先 UNIX 中的代码，于是 UCB 重写了 BSD 中所有来自于 UNIX 的代码，发布了一个全新的版本，称为 4.4 BSD-Lite。



参考：

- [什么是 UNIX 以及它为什么这么重要？| Linux 中国](https://linux.cn/article-3452-1.html)
- [鳥哥的 Linux 私房菜 - Linux 之前，Unix 的歷史](https://linux.vbird.org/linux_basic/centos7/0110whatislinux.php#whatislinux_unix)


如果你想要了解更多关于计算机科学的信息，强烈推荐系列视频：[Crash Course Computer Science | YouTube](https://youtube.com/playlist?list=PL8dPuuaLjXtNlUrzyH5r6jN9ulIgZBpdo&si=QlcZDwmadQOJGafR)