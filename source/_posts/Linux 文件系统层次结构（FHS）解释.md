详细的目录解释可以使用 `man file-hierarchy` 和 `man hier` 命令查看。

常见目录：

| 目录 | 描述 |
| ------------ | ------------ |
| `/bin` | binaries。在单用户模式下需要用到的基本命令的二进制文件，包括启动系统或修复系统所需的命令，所有用户都可以使用。目前 `/bin` 目录大多被链接到 `/usr/bin` 了 |
| `/sbin` | system binaries。系统必要的二进制文件，系统管理员可用。目前 `/sbin` 目录大多被链接到 `/usr/sbin` 了 |
| `/usr` | UNIX shared resources，只读用户数据的二级层次结构，包含大多数用户工具和应用。应该是可共享和只读的。 |
| `/usr/bin` | 非必要命令的二进制文件，所有用户都可以使用。 |
| `/usr/sbin` | 非必要命令的系统二进制文件。系统管理员可用。 |
| `/usr/lib` | `/usr/bin` 和 `/usr/sbin` 中二进制文件的库文件。 |
| `/usr/lib32` | 替代格式库文件，在 64 位机器上提供 32 位库文件。 |
| `/usr/libexec` | 由其他程序运行的二进制文件，不打算由用户或 shell 脚本直接执行。 |
| `/usr/share` | 存储与架构无关的共享资源，如字体文件 |
| `/usr/include` | 标准 include 文件。如 stdio.h |
| `/usr/src` | 源代码（例如，带有头文件的内核源代码） |
| `/usr/local` | 本地数据的第三级层次结构，特定于主机。通常具有进一步的子目录（例如，`bin`，`lib`，`share`） |
| `/usr/local/bin`| 用于存放通过源代码编译安装或非标准包管理器安装的软件的可执行文件 |
| `/usr/local/etc` | 存储在`/usr/local`目录下安装的软件的配置文件 |
| `/var` | variable。变量文件：这些文件的内容在系统运行期间会不断变化，例如日志、spool 文件和临时电子邮件文件。 |
| `/var/log` | 日志文件 |
| `/var/lib` | 状态信息。程序运行时被程序修改的持久化数据（例如数据库、打包系统元数据等）。MySQL 的数据库文件的存储位置就在 `/var/lib/mysql` |
| `/var/opt` | 存储在 `/opt` 中的附加软件包的变量数据。 |
| `/var/tmp` | 重启之间需要保留的临时文件。 |
| `/etc` | Editable Text Configuration（名称存在争议）。存储系统范围的配置文件 |
| `/etc/opt` | 存储在 `/opt` 中的附加软件包的配置文件。 |
| `/mnt` | mount。临时挂载的文件系统。 |
| `/dev` | device。设备文件（例如 `/dev/null`，`/dev/disk0`，`/dev/sda1`，`/dev/tty`，`/dev/random`） |
| `/opt` | optional。附加应用软件包。和 `/usr/local/bin` 的区别在于 `/opt` 往往用来存放大型软件包，每个软件包可以拥有自己独立的目录，如 `/opt/<application>` |
| `/sys` | 包含有关设备、驱动程序和一些内核功能的信息。 |
| `/proc` | 是一个虚拟的文件系统，将进程和内核信息以文件的形式提供。在 Linux 中，对应于 procfs 挂载。通常由系统自动生成并动态填充。 |
| `/tmp` | 临时文件目录（另请参阅 `/var/tmp` ）。通常在系统重新启动之间不会保留，并且可能受到严格的大小限制。 |

> [/tmp 和 /var/temp 的关系](https://zhuanlan.zhihu.com/p/352642228)

## FHS

文件系统层次结构标准（FHS）是描述类 Unix 系统层次结构惯例的参考文献。它因在 Linux 发行版中的使用而广为人知，但其他类 Unix 系统也在使用。它由 Linux 基金会维护。最新版本是 [FHS 3.0](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)，于 2015 年 6 月 3 日发布。

一些发行版通常遵循标准，但在某些领域有所偏离。FHS 是一个“尾随标准”，因此记录了某一时刻的常见做法。当然，时代在变化，发行版的目标和需求需要进行实验。一些常见的偏离包括：

现代 Linux 发行版包括一个 `/sys` 目录作为虚拟文件系统（sysfs，类似于 `/proc` ，它是一个procfs），用于存储和允许修改连接到系统的设备，而许多传统的类 Unix 操作系统使用 `/sys` 作为指向内核源代码树的符号链接。

许多现代类 Unix 系统（如 FreeBSD 通过其 ports 系统）将第三方软件包安装到 `/usr/local` 中，同时将被视为操作系统一部分的代码保留在 `/usr` 中。

一些 Linux 发行版不再区分 `/lib` 和 `/usr/lib` ，并将 `/lib` 符号链接到 `/usr/lib`。

一些 Linux 发行版不再区分 `/bin` 和 `/usr/bin` ，以及 `/sbin` 和 `/usr/sbin` 之间的区别。它们可能会将 `/bin` 符号链接到 `/usr/bin`，将 `/sbin` 符号链接到 `/usr/sbin`。其他发行版选择将所有四个整合在一起，将它们符号链接到 `/usr/bin`。

参考：[File Hierarchy Standard | Wikipedia](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)