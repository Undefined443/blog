## 简介

要建立云服务器，首先需要安装操作系统。在现代环境中，几乎所有情况下都是指 Linux 操作系统。从历史上看，Windows 服务器和其他类型的 Unix 在特定的商业环境中都很流行，但现在几乎每个人都在运行 Linux，这是因为 Linux 支持广泛、许可免费或灵活，而且在服务器计算领域总体上无处不在。Linux 有许多发行版，每个发行版都有自己的维护者，有些发行版有商业供应商支持，有些则没有。以下各节详细介绍的发行版是运行云服务器的一些最流行的操作系统。

## 概述

**Ubuntu** 是最流行的 Linux 发行版之一，既适用于服务器，也适用于台式电脑。新的 Ubuntu 版本每六个月发布一次，新的 Ubuntu 长期支持版本每两年发布一次，支持期为五年。由于 Ubuntu 广受欢迎，大多数有关 Linux 的教育内容都反映了 Ubuntu 的特点，而 Ubuntu 支持的广泛性也是对其有利的重要一点。

**Debian** 是 Ubuntu 的上游，这意味着它的核心架构决定通常会影响 Ubuntu 的发布，它使用与 Ubuntu 相同的 `.deb` 包格式和 `apt` 包管理器。由于保守的打包选择和缺乏商业支持，Debian 在生产服务器上并不流行。不过，许多用户选择 Debian 是因为它的可移植性，以及它被用作不同平台上许多其他 Linux 发行版的基线，包括最流行的 Raspberry Pi 操作系统 Raspbian。

**Red Hat Enterprise Linux** 或 RHEL 是最流行的商业支持 Linux 发行版。与 Debian 系列不同，它使用 `.rpm` 软件包和名为 `dnf` 的软件包管理器，以及自己的工具生态系统。由于授权原因，只有在签订了商业支持协议的情况下，才会使用 Red Hat。

**Rocky Linux** 是 Red Hat 的下游，就像 Ubuntu 是 Debian 的下游一样，与 RHEL 不同的是，它与大多数其他 Linux 发行版一样可以免费使用，这使它成为采用红帽工具但可能不使用红帽商业支持的用户的热门选择。在此之前，一个名为 CentOS 的发行版扮演着与 Rocky Linux 相同的角色，但它的发布模式正在发生变化。Rocky Linux 的版本与 RHEL 的版本密切相关，两者之间可以共享大多数文档。

**Fedora Linux** 是 Red Hat 的上游产品，与 Ubuntu 一样，用于桌面环境和服务器。Fedora 是大多数 RHEL 生态系统软件包以及 Gnome 桌面环境（Ubuntu 和其他系统默认使用 Gnome 桌面环境）的实际开发源头。

**Arch Linux** 是另一种流行的以桌面为重点的 Linux 发行版，它既不是 Debian 也不是 Red Hat Linux 系列的成员，但提供了自己独特的打包格式和工具。与其他发行版不同的是，它不使用任何发行版本--它的软件包总是最新的。因此，不建议用于生产服务器，但它提供了出色的文档，对知识渊博的用户来说非常灵活。

**Alpine Linux** 是一个最小的 Linux 发行版，默认情况下不提供许多常用工具。从历史上看，许多 Linux 发行版都是基于这一目标而创建的。Alpine 常用于现代容器化部署（如 Docker），在这种部署中，你的软件可能需要在虚拟化操作系统中运行，但又需要尽可能减少整体占用空间。除非试图制作容器原型，否则一般不会直接使用 Alpine Linux。

以前，不同发行版在初始系统、窗口管理器和其他库的选择上存在较大差异，但现在几乎所有主流 Linux 发行版都已将 systemd 和其他此类工具标准化。

## 选择发行版

还有许多其他 Linux 发行版，但大多数其他发行版目前都可以通过这七个发行版来理解。从以上概述可以看出，你选择 Linux 发行版的大部分标准将归结为以下几点：

- 您对 Debian 衍生系统还是 Red Hat 生态系统有要求
- 你将主要为云、桌面或容器进行开发
- 是需要使用最新的可用软件包，还是稳定的软件包

选择发行版取决于个人喜好，但如果你是在云中工作，对红帽生态系统没有任何生产要求，那么 Ubuntu 是最受欢迎的默认选择。您还可以从面向网站的软件包仓库中查看特定发行版的可用软件包。例如，Ubuntu 22.04 "Jammy Jellyfish" 软件包位于 [Ubuntu.com 的 Jammy 部分](https://packages.ubuntu.com/jammy/)。

## 软件包管理

大多数 Linux 发行版在如何创建、发现和安装第三方软件包（软件包源中没有的软件包）方面也有很大不同。Red Hat、Fedora 和 Rocky Linux 除了官方软件包外，一般只使用少数几个流行的第三方软件包库，以保持其权威性和生产性。其中之一就是[企业 Linux 额外软件包](https://docs.fedoraproject.org/en-US/epel/)（Extra Packages for Enterprise Linux 或 EPEL）。由于 RHEL 生态系统对商业上支持的软件包和不支持的软件包进行了区分，所以许多在 Ubuntu 上开箱即用的常用软件包需要配置 EPEL 才能安装到 Red Hat 上。在这种情况和其他许多情况下，哪些软件包可以从发行版自己的软件源上游获得，往往是一个权威性和维护责任的问题。许多第三方软件包源都广受信任，只是它们可能不在发行版维护者的职责范围之内。

Ubuntu 允许个人用户创建 PPA（即个人软件包档案）来维护第三方软件供他人安装。不过，同时使用过多的 PPA 可能会导致不兼容的问题，因为 Debian 和 Ubuntu 软件包的版本都有特定的要求，所以 PPA 维护者需要与 Ubuntu 的上游更新紧密配合。Arch Linux 为用户提交的软件包提供了一个单一的软件仓库，称为 Arch User Repository 或 AUR，虽然相比之下，他们的方法显得更加混乱，但如果你使用几十个第三方软件包，这种方法在实际操作中会更加方便。

你也可以通过 [Homebrew](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-homebrew-on-linux) 或 [Docker](https://www.digitalocean.com/community/tutorial_collections/how-to-install-and-use-docker) 安装第三方软件，从而避免增加系统软件包管理器的复杂性。虽然"Docker 化"或容器化部署在磁盘使用和安装开销方面可能效率不高，这也是 Alpine Linux 需要考虑的地方，但它们可以跨发行版移植，而且不会对你的系统提出任何版本要求。不过，任何未被系统软件包管理器安装的软件包在默认情况下可能无法接收自动更新，这也是需要考虑的另一个问题。

## 总结

在本教程中，你回顾了为云计算选择 Linux 发行版时的一些最重要的考虑因素。现在，Docker 和其他容器引擎的广泛使用意味着，选择发行版对你能够运行的软件的影响已不像过去那么大了。不过，这仍然是影响软件获得支持方式的重要因素，也是您在为生产扩展基础架构时需要考虑的重要因素。

要进一步了解如何在不同的 Linux 发行版上使用系统软件包管理器，请参阅[《软件包管理要点》](https://www.digitalocean.com/community/tutorials/package-management-basics-apt-yum-dnf-pkg)。

参考：[How to Choose a Linux Distribution | DigitalOcean Tutorial](https://www.digitalocean.com/community/conceptual-articles/how-to-choose-a-linux-distribution/)