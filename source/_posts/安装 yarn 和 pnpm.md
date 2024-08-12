yarn 2 和 pnpm 都是使用 Corepack 管理的，Corepack 是一个管理包管理器的工具。你可以在 [Corepack 官网](https://nodejs.org/api/corepack.html)查看相关介绍。

首先启用 Corepack：

```sh
corepack enable
```

### [Yarn 2](https://yarnpkg.com/)

使用 yarn 2 初始化项目：

```sh
yarn init -2
```

常用命令：

```sh
yarn set version stable # 更新 yarn 为最新的稳定版
yarn install            # npm install
yarn add <package>      # npm install <package>
```

参考：[Install yarn](https://yarnpkg.com/getting-started/install)

### [pnpm](https://pnpm.io/)

```sh
corepack prepare pnpm@latest --activate
```

常用命令：

```sh
pnpm install   # npm install
pnpm add <pkg> # npm install <package>
pnpm <cmd>     # npm run <cmd>
```

参考：[安装 pnpm](https://pnpm.io/zh/installation)