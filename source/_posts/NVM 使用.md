[NVM](https://github.com/nvm-sh/nvm)（Node Version Manager）是 Node.js 的版本管理工具。

NVM 项目为 macOS 和 Linux 开发。Windows 用户需要使用 [NVM for Windows](https://github.com/coreybutler/nvm-windows)。

## 安装

NVM 官方不建议使用 Homebrew 管理 NVM。

**macOS / Linux**

```sh
# 安装 NVM
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
# 安装 LTS 版本的 Node.js
nvm install lts
```

如果稍后要更新 NVM，再次运行 NVM 安装命令即可。

> 参考：[Install & Update Script | README](https://github.com/nvm-sh/nvm?tab=readme-ov-file#install--update-script)

**Windows**

```powershell
# 安装 NVM
winget install CoreyButler.NVMforWindows
# 安装 LTS 版本的 Node.js
nvm install lts
```

> 参考 [Install nvm-windows](https://github.com/coreybutler/nvm-windows#install-nvm-windows)

## 常用命令

| 命令                           | 说明                                                 |
| ------------------------------ | ---------------------------------------------------- |
| `nvm install <version>`   | 安装指定版本的 Node.js。                             |
| `nvm uninstall <version>` | 删除指定版本的 Node.js。                             |
| `nvm use <version>`       | 切换使用指定的版本。                                 |
| `nvm ls-remote --lts`          | 列出所有官方的 Node.js 版本。                        |
| `nvm ls`                       | 列出所有安装的 Node.js 版本。                        |
| `nvm current`                  | 显示当前使用的版本。                                 |
| `nvm alias`                    | 给不同版本添加别名。                                 |
| `nvm unalias`                  | 删除自定义的别名。                                   |
| `nvm reinstall-packages`       | 在当前 Node.js 环境下，重新安装指定版本号的 npm 包。 |

默认别名：

- `default`：默认启用的 Node.js 版本。
- `node`：通常指向最新的稳定版本的 Node.js。当你运行 `nvm install node` 时，NVM 会安装最新的稳定版，并将其作为 `node` 别名。
- `stable`：通常指向最新的稳定版本的 Node.js，与 `node` 类似。
- `lts/*`：指向最新的长期支持（LTS）版本的 Node.js，例如 `lts/argon`。
- `lts/argon`、`lts/boron` 等：指向特定代号的 LTS 版本。

```sh
nvm install node     # 安装最新的稳定版本的 node
nvm install 16       # 安装最新版的 node 16
nvm alias default 16 # 设置默认使用 node 16
```

> node 版本选择：对于老项目，最好使用 `16.x.x` 版本。更高级的版本有很大的兼容性问题。

## Troubleshooting

在 Windows 上执行 `nvm use <version>` 命令出现乱码：原因是权限不足。请在管理员权限下重新运行命令。