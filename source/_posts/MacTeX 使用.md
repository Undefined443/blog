MacTeX 是一个 TeX Live 的 macOS 定制版本。它包括：

- TeX Live
- GUI 应用程序
- Ghostscript

关于 MacTeX 的介绍可以查看 [MacTex 主页](https://www.tug.org/mactex/)

## 安装

```sh
brew install --cask mactex
```

如果你不使用 MacTeX 的 GUI 工具，比如说你想用 VS Code 编辑 `.tex` 文件，可以安装无 GUI 版本：

```sh
brew install --cask mactex-no-gui
```

> 这个版本只包含 MacTeX 命令行工具

MacTeX 有一个生成数学公式矢量图的小工具 LaTeXiT，可以单独安装：

```sh
brew install --cask latexit
```

## 编译 LaTeX 文档

### 使用命令行工具

如果要使用命令行工具进行编译，只需使用 `latexmk`：

```sh
latexmk main.tex
```

编译之后将得到 `main.dvi` 文件，你可以使用 [Skim](https://sourceforge.net/projects/skim-app/files/Skim/Skim-1.7.1/Skim-1.7.1.dmg/download) 查看 `.dvi` 文件。

如果你想要得到 PDF 文件，使用下面的命令：

```sh
latexmk -pdf main.tex
```

如果你要使用 XeLaTeX 引擎进行编译，需要添加 `-xelatex` 选项：

```sh
latexmk -xelatex main.tex
```

如果 `.tex` 文件中使用的宏包要执行外部程序，需要添加 `-shell-escape` 选项：

```sh
xelatexmk -shell-escape main.tex
```

清理临时文件：

```sh
latexmk -c  # 不清理 PDF 以及其他一些最终文件
latexmk -C  # 清理所有文件（包括 PDF）
```

常用命令：

```sh
latexmk \
    -synctex=1 \
    -interaction=nonstopmode \
    -file-line-error \
    -xelatex \
    -shell-escape \
    -outdir=build \
    main.tex
```

### 与 Visual Studio Code 集成

VS Code 有一款名为 [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop) 的插件，可以很好地与 TeX Live 工具集成。

关于 LaTeX Workshop 插件的用法，网上的相关文档已经很多了。这里只记录一些个人的插件配置。

```json
// 关闭公式鼠标悬停预览
 "latex-workshop.hover.preview.enabled": false,

// 在智能感知功能中使用 biblatex 包作为参考文献和引文处理的后端
"latex-workshop.intellisense.citation.backend": "biblatex",

// 启用包智能提示
"latex-workshop.intellisense.package.enabled": true,

// 设置编译输出目录，避免输出文件污染根目录
"latex-workshop.latex.outDir": "%DIR%/build",

// 设置默认编译链为上次使用的编译链
"latex-workshop.latex.recipe.default": "lastUsed",

// 编译链，我只需要 latexmk (xelatex) 和 latexmk
"latex-workshop.latex.recipes": [
        {
            "name": "latexmk (xelatex)",
            "tools": ["xelatexmk"]
        },
        {
            "name": "latexmk",
            "tools": ["latexmk"]
        }
    ],
// 编译工具，加入 -shell-escape 选项
"latex-workshop.latex.tools": [
    {
        "name": "latexmk",
        "command": "latexmk",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "-pdf",
            "-shell-escape",
            "-outdir=%OUTDIR%",
            "%DOC%"
        ],
        "env": {}
    },
    {
        "name": "xelatexmk",
        "command": "latexmk",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "-xelatex",
            "-shell-escape",
            "-outdir=%OUTDIR%",
            "%DOC%"
        ],
        "env": {}
    }
],

// 关闭编译错误提示
"latex-workshop.message.error.show": false,
"latex-workshop.message.warning.show": false,

// 在 VS Code 右键上下文菜单中显示与 LaTeX Workshop 相关的选项
"latex-workshop.showContextMenu": true,

// 设置正向查找快捷键为双击鼠标左键
"latex-workshop.view.pdf.internal.synctex.keybinding": "double-click",
```

快捷键：

- 编译：`⌘` `⌥` `B`
- 预览：`⌘` `⌥` `V`
- 正向搜索：`⌘` `⌥` `J`
- 反向搜索：`⇧` `⌘` 单击

参见：

- [「MacTeX 小笔记」如何使用 LaTeXiT，以及导出一个数学公式图片 | CSDN](https://blog.csdn.net/qq_33919450/article/details/124456682)

- [MacTeX 安装 xeCJK 和 CTEX 搭建中文环境 | 知乎](https://zhuanlan.zhihu.com/p/35094852)

## 相关工具介绍

### MacTeX GUI

如果你安装了 MacTeX 的 GUI 版本，你会获得下列 5 款工具：

- TeXShop: TeX/LaTeX 编辑器
- BibDesk: 参考文献管理工具
- LaTeXiT: 一种小巧的公式编辑器
- hintview: HINT 文件查看器
- TeX Live Utility: TeX Live 的包管理器（其实就是 tlmgr 的图形前端）

#### LaTeXiT

[LaTeXiT](https://www.chachatelier.fr/latexit/latexit-home.php) 是一款方便小巧的公式渲染工具。通过输入 LaTeX 代码，它可以为我们生成公式预览并能够以多种格式导出（PDF、SVG、PNG 等等）。

LaTeXiT 工具的主界面可以选择四种渲染模式。下面介绍一下这几种模式的区别：

##### Align 模式

在 Align 模式书写代码相当于在 LaTeX 文档的 `align*` 块中书写代码：

```tex
\begin{align*}
    a + b & = c\\
          & = d
\end{align*}
```

![image](https://s2.loli.net/2024/07/07/ICVNgiXd9O4GJF7.png)

##### Display 模式

在 Display 模式书写代码相当于在 LaTeX 文档的 `$$...$$` 行间代码块中书写代码：

```tex
$$
    \sum_{i=1}^n a_i = \frac{n}{2} \left(a_1 + a_n\right)
$$
```

![image](https://s2.loli.net/2024/07/07/lfokr7KWHSsbUdy.png)

##### Inline 模式

在 Inline 模式书写代码相当于在 LaTeX 文档的 `$...$` 行内代码块中书写代码：

```tex
$\sum_{i=1}^n a_i = \frac{n}{2} \left(a_1 + a_n\right)$
```

![image](https://s2.loli.net/2024/07/07/BykbhA5CmcjszFH.png)

##### Text 模式

在 Text 模式书写代码相当于直接书写 LaTeX 代码：

```tex
你好，\LaTeX ！
```

![image](https://s2.loli.net/2024/07/07/RwsOVylE9kIn6C2.png)

#### hintview

HINT 文件是一种可重排的 TeX 输出文件，它在预览时可以像 HTML 网页一样对字体放大、缩小。

使用 `hitex` 和 `hilatex` 工具构建 HINT 文件。

更多关于 HINT 文件：[HiTEX: Reflowable Output for TEX](https://hint.userweb.mwn.de/hint/hitex.html)

### MacTeX CLI

1. TeX 引擎:

   - tex — 原始的 TeX 引擎。
   - latex — LaTeX 引擎，使用 DVI 格式输出。
   - pdflatex — LaTeX 引擎，直接生成 PDF 文档。
   - xelatex — 支持现代字体技术（如 Unicode 和 OpenType）的 LaTeX 引擎。
   - lualatex — 集成了 Lua 脚本语言的 LaTeX 引擎。

2. BibTeX 相关工具:

   - bibtex — BibTeX，生成参考文献列表的工具。
   - biber — BibTeX 的替代品，与 biblatex 宏包配合使用。

3. 索引生成工具:

   - makeindex — 生成索引列表的工具。
   - xindy — 生成索引的工具，用于更复杂的索引排版。

4. 格式转换工具:

   - dvips — 将 DVI 格式文件转换为 PostScript 文件。
   - dvipdfmx — 将 DVI 格式文件转换为 PDF 文件。
   - ps2pdf — 将 PostScript 转换为 PDF。
   - dvi2tty — 将 DVI 文件显示为ASCII文本。
   - gs — PostScript 和 PDF 解析和渲染工具

5. 字体相关工具:

   - kpsewhich — 文件查找工具，用于查找 TeX 系统中的文件。
   - updmap — 更新字体映射文件的工具。
   - mktexlsr 或 texhash — 更新 TeX 目录索引数据库。

6. LaTeX 宏包和文档管理工具:

   - tlmgr — TeX Live 管理器，用于安装和更新 TeX Live 宏包。

7. 其他工具:

   - texdoc — 查看 TeX 相关文档的工具。
   - texconfig — TeX 配置工具。
   - mpost — 图形编程语言，用来创建矢量图形。
   - pdfcrop — PDF 处理工具，可以将 PDF 的白边裁剪掉只留下核心内容。

## Troubleshooting

### LaTeXiT

打开 LaTeXiT 时，可能遇到如下问题：

1. 软件语言是法语

   解决方法：打开系统设置（没错，是电脑的系统设置，不是软件自身的设置），打开 `通用 > 语言与地区`，在 `首选语言` 中添加 `English` 作为备选语言：

   ![image](https://s2.loli.net/2024/07/07/MArH2cxWVPkDfG7.png)

2. 软件提示找不到 `Ghostscript` 和 `ps2pdf`

   解决方法：按下 <kbd>⌘</kbd> + <kbd>,</kbd> 进入设置，打开 Typesetting 标签页，然后填写 Ghostscript (gs) 以及 ps2pdf 的位置。

   如果你不知道 gs 和 ps2pdf 的位置，可以打开终端，输入 `which gs` 和 `which ps2pdf` 来查找。

   ![image](https://s2.loli.net/2024/07/07/OB5qIQhJWuKN8kC.png)

3. 无法导出 SVG

   如果你在导出 SVG 时看到提示：

   ```
   Warning: pdf2svg or pdftocairo was not found
   ```

   说明程序找不到 pdf2svg 或者 pdftocairo 的路径。打开 `File > Export image...`（`⌘` + `E`），选择 Format 为 SVG vector format，然后看到右边有一个 “⌥s...” 按钮，点开它，就可以配置 pdf2svg 和 pdftocairo 的路径了。

   ![image](https://s2.loli.net/2024/07/07/JL7TkDCBs63ywuU.png)

   ![image](https://s2.loli.net/2024/07/07/NVl3gaE7vbKWRPy.png)

4. 无法渲染中文

   首先在设置中打开 Templates，新增一个 Preamble 模板，名称可以起为 `ctex`：

   ```tex
   \documentclass[fontset=founder]{ctexart}
   \usepackage{xcolor} % used for font color
   \usepackage{amssymb} % maths
   \usepackage{amsmath} % maths
   ```

   ![image](https://s2.loli.net/2024/07/07/6ueF5PoTRijXkzy.png)

   然后打开 Typesetting，选用 `xelatex` 设置：

   ![image](https://s2.loli.net/2024/07/07/Oha85PlLjGH7fMQ.png)

   接下来，可以试着渲染一段含有中文的 LaTeX 代码：

   ![image](https://s2.loli.net/2024/07/07/bwGvyZKma32MkVL.png)

### TeX Live Utility

使用 TeX Live Utility 时可能遇到的问题是，如果你挂着梯子使用 TeX Live Utility，那么有几率 TeX Live Utility 会卡死（实际上是在加载网络资源）。建议使用 TeX Live Utility 时不要挂梯子。

### Pygments

编译使用了 `pygments` 包的文档时报错：

```
Package minted: Missing Pygments output; \inputminted was probably given a file that does not exist--otherwise, you may need the outputdir package option, or may be using an incompatible build tool, or may be using frozencache with a missing file.
```

如果你在使用 `latexmk` 编译 `.tex` 文件时添加了 `-outdir` 选项，那么你在引入 `minted` 包时要添加 `outputdir=<outdit>` 参数：

```latex
\usepackage[outputdir=.output]{minted}
```
