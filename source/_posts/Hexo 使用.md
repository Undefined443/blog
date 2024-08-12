Hexo 是一个静态博客站点生成工具，可以将 Markdown 格式的文档转换成静态页面，非常适合用来做个人技术博客。

## 常用命令

### 建站

```sh
npx hexo-cli init blog # 初始化一个博客站
npx hexo-cli server    # 启动博客站，用于调试
npx hexo-cli generate  # 生成静态页面到 public 目录
npx hexo-cli deploy    # 一键部署
```

静态页面目录 `public` 下的文件可以放到 Nginx 配置目录下作为一个站点。

### 写作

```sh
npx hexo-cli new <title>     # 创建 Markdown 文件
vim source/_posts/<title>.md # 编辑 Markdown 文件
```

## Hexo + GitHub Pages 博客

1. 使用 `hexo-cli` 初始化一个项目。

   ```sh
   npx hexo-cli init REPONAME  # REPONAME 可随意修改
   ```

2. 修改 Hexo 配置文件。

   编辑 `_config.yml`，将这下面段配置中的 URL 改为 `https://USERNAME.github.io/REPONAME`。其中 `USERNAME` 是你的 GitHub 用户名，`REPONAME` 是当前库名。

   ```yaml
   # 将这行
   url: http://example.com
   # 替换为
   url: https://USERNAME.github.io/REPONAME
   ```

3. 创建 GitHub Actions 配置文件

   编辑 `.github/workflows/pages.yml`：

   ```yaml
   name: Pages

   on:
     push:
       branches:
         - main # default branch

   jobs:
     build:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v4
           with:
             token: ${{ secrets.GITHUB_TOKEN }}
             # If your repository depends on submodule, please see: https://github.com/actions/checkout
             submodules: recursive
         - name: Use Node.js 20
           uses: actions/setup-node@v4
           with:
             # Examples: 20, 18.19, >=16.20.2, lts/Iron, lts/Hydrogen, *, latest, current, node
             # Ref: https://github.com/actions/setup-node#supported-version-syntax
             node-version: "20"
         - name: Cache NPM dependencies
           uses: actions/cache@v4
           with:
             path: node_modules
             key: ${{ runner.OS }}-npm-cache
             restore-keys: |
               ${{ runner.OS }}-npm-cache
         - name: Install Dependencies
           run: npm install
         - name: Build
           run: npm run build
         - name: Upload Pages artifact
           uses: actions/upload-pages-artifact@v3
           with:
             path: ./public
     deploy:
       needs: build
       permissions:
         pages: write
         id-token: write
       environment:
         name: github-pages
         url: ${{ steps.deployment.outputs.page_url }}
       runs-on: ubuntu-latest
       steps:
         - name: Deploy to GitHub Pages
           id: deployment
           uses: actions/deploy-pages@v4
   ```

   将文件中的 `20` 改为你实际使用的 Node.js 的版本。

4. 在 GitHub 上建立同名仓库 `REPONAME`。

5. 编辑仓库配置，打开 `Settings` > `Pages`，并将 `Source` 改为 `GitHub Actions`。

6. 将本地项目上传到 GitHub。

7. 打开 `https://USERNAME.github.io/REPONAME` 查看效果。

参考：[Hexo 文档](https://hexo.io/zh-cn/docs/)