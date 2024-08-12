## [npm](https://www.npmjs.com/)

### 常用命令

```sh
# 初始化项目，生成 package.json 文件
npm init

# 安装 package.json 中的依赖
npm install

# 安装生产依赖（默认）
npm install <package> --save
npm install <package> -S

# 安装开发依赖
npm install <package> --save-dev
npm install <package> -D
```

[node 安装依赖时带 --save-exact 是为什么 | CSDN](https://blog.csdn.net/aaqingying/article/details/121209404)

### 换源

npm 换源有多种方法

#### 使用中国 npm 镜像（建议）

```sh
# 设置永久镜像源
npm config set registry https://registry.npmmirror.com/
# 也可以临时使用镜像源安装一个包
npm install <package> --registry=https://registry.npmmirror.com
```

> 你也可以为只单个项目指定镜像源，在项目根目录下创建文件 `.npmrc` 并填写配置

#### 使用镜像管理工具 nrm

```sh
# 安装 nrm
npm install -g nrm
# 设置镜像源
nrm add npmmirror https://registry.npmmirror.com
nrm add official https://registry.npmjs.org
# 切换镜像源
nrm use npmmirror
```

#### 使用淘宝镜像源包管理器 `cnpm`

```sh
# 安装 cnpm
npm install -g cnpm --registry=https://registry.npmmirror.com
# 使用 cnpm 安装包
cnpm install <package>
```

> `cnpm` 和 `npm` 不要混用
>
> 最好换源而不是使用 `cnpm`

### Troubleshooting

#### Only Mac 64 bits supported

```
npm ERR! code 1
npm ERR! path /Users/xxx/node_modules/chromedriver
npm ERR! command failed
npm ERR! command sh -c node install.js
npm ERR! Only Mac 64 bits supported.
```

解决方法：

```sh
npm install --ignore-scripts
```

参考：[解决 npm install 时报错无法安装 chromedriver 的问题 | 51CTO 博客](https://blog.51cto.com/zero01/2298070)

#### NPM 代理问题

```
npm ERR! code ECONNRESET
npm ERR! errno ECONNRESET
npm ERR! network request to https://registry.npmjs.org/string_decoder/-/string_decoder-0.10.31.tgz failed, reason: Client network socket disconnected before secure TLS connection was established
npm ERR! network This is a problem related to network connectivity.
npm ERR! network In most cases you are behind a proxy or have bad network settings.
npm ERR! network 
npm ERR! network If you are behind a proxy, please make sure that the
npm ERR! network 'proxy' config is set properly.  See: 'npm help config'

npm ERR! A complete log of this run can be found in:
npm ERR!     /home/xxx/.npm/_logs/2022-10-22T11_00_36_894Z-debug-0.log
```

解决方法：关闭终端代理，或者设置 NPM 代理，或者使用 HTTP (而不是 HTTPS) 连接 package 库（不建议）：

```sh
# 设置 npm 代理
npm config set proxy=http://127.0.0.1:7890
# 或者使用 HTTP 连接（不建议）
npm config set registry=http://registry.npmjs.org
```

> 最好设置镜像源而不是设置代理

## npx

`npx` 是一个随 `npm` 一起发布的命令行工具。`npx` 的设计初衷是为了简化使用 `npm` 包的一些常见任务，尤其是那些不需要全局安装的命令行工具。

1. 比如，有时你可能只想在项目中安装一个包，而不希望全局安装。使用 `npx` 可以直接运行本地安装的包，而无需手动指定路径。

   例如，如果你在项目中本地安装了 `ts-node`：

   ```sh
   npm install ts-node
   ```

   你可以使用 `npx` 运行 `ts-node` 而不需要全局安装它：

   ```sh
   npx ts-node main.ts
   ```

2. 或者有时你可能只需要运行一次某个工具，而不希望安装它。`npx` 会临时下载并运行该工具，然后删除它。

   例如，你想检查一个包的版本，但不想安装 `npm-check`：

   ```sh
   npx npm-check
   ```

3. `npx` 还支持运行远程代码片段，例如 GitHub Gist：

   ```sh
   npx gist.github.com/username/gistid
   ```