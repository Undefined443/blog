### 介绍

~~LaTeX 原生不支持中文。为了添加中文的功能，我们需要引入宏包。~~XeLaTeX 原生支持中文。不过由于默认使用的字体是英文字体，我们需要设置中文字体之后才能用。不过由于一些原因，在使用 LaTeX 书写中文文档的时候最好还是使用 XeLaTeX 引擎搭配 ctex 文档类或者 ctex 宏包。

支持中文字体的宏包有 [`CJK`](https://www.ctan.org/pkg/cjk)、[`xeCJK`](https://www.ctan.org/pkg/xecjk) 和 [`ctex`](https://ctan.org/pkg/ctex)。

- 其中 `CJK` 是最古老的，其对中文字体的支持比较麻烦，不推荐使用。
- `xeCJK` 以及 `luatexja` 宏包在 `CJK` 基础上封装了对汉字排版细节的处理功能。
- `ctex` 宏包和文档类进一步封装了 `CJK`、`xeCJK`、`luatexja` 等宏包，使得用户在排版中文时不再考虑排版引擎等细节。

所以目前来说书写中文 LaTeX 文档的最优解决方案是使用 `ctex` 文档类或者 `ctex` 宏包。

### 使用 ctex 文档类

如果你要撰写一篇纯中文的文档，那么你可以直接使用 `ctex` 文档类：

```tex
\documentclass[fontset=founder]{ctexart}
```

### 使用 ctex 宏包或者 xeCJK 宏包

如果你需要使用特定的文档类而不能使用 `ctex` 文档类，但又要在文档中插入中文，那么你可以使用 `ctex` 宏包或者 `xeCJK` 宏包：

```tex
\usepackage[fontset=founder]{ctex}
```

```tex
\usepackage{xeCJK}
```

`xeCJK` 对中文的支持更朴素一点，也就是它除了支持你插入中文以外不会改变文档其他地方。而 `ctex` 则会将文档类中的一些英文环境也翻译成中文。相对来说更推荐使用 `ctex`。

使用 xeCJK：

```tex
\documentclass{article}
\usepackage{fontspec} % 加载 fontspec 宏包
\usepackage{xeCJK} % 加载 xeCJK 宏包

\setmainfont{Times New Roman} % 设置英文字体
\setCJKmainfont{SimSun} % 设置中文字体

\begin{document}
这是中文文本。This is English text.
\end{document}
```

使用 ctex：

```tex
\documentclass{article}
\usepackage[fontset=founder]{ctex}

\setmainfont{Times New Roman} % 设置英文字体
\setCJKmainfont{SimSun} % 设置中文字体

\begin{document}
这是中文文本。This is English text.
\end{document}
```

### 在 XeLaTeX 中直接使用中文

XeLaTeX 引擎原生支持 Unicode 字符，因此我们实际上可以直接使用中文书写 LaTeX 文档。不过前提是设置一下中文字体，因为虽然 XeLaTeX 引擎原生支持中文字符，但是其使用的默认字体是英文字体，不包括中文字符。

```tex
\documentclass{article}

\usepackage{fontspec} % 使用 fontspec 包配置字体
\setmainfont{SimSun} % 设置宋体为主要字体

\begin{document}
你好，\LaTeX
\end{document}
```

### 附录

`ctex` 预定义的中文字库：

| 字体选项    | 描述                                                    | 支持pdfLATEX                  |
|:-----------:|:--------------------------------------------------------|:-----------------------------|
| `adobe`     | 使用 Adobe 公司的四款中文字体                           | 不支持                       |
| `fandol`    | 使用 Fandol 中文字体                                    | 不支持                       |
| `founder`   | 使用方正公司的中文字体                                  | 支持                         |
| `mac`       | 使用 macOS 系统下的字体，分为 `macnew` 和 `macold` 两种    | 不支持                       |
| `macnew`    | 使用 El Capitan 或之后的多字重华文字体和苹方字体         | 支持                         |
| `macold`    | 使用 Yosemite 或之前的华文字体                          | 支持                         |
| `ubuntu`    | 使用 Ubuntu 系统下的思源宋体、思源黑体和文鼎楷体         | 不支持                       |
| `windows`   | 使用 Windows 系统下的中易字体和微软雅黑字体             | 特定条件下支持（见描述）     |

`ctex` 中定义的字体命令：

- `\songti`：宋体，CJK 等价命令 `\CJKfamily{zhsong}`。
- `\heiti`：黑体，CJK 等价命令 `\CJKfamily{zhhei}`。
- `\fangsong`：仿宋，CJK 等价命令`\CJKfamily{zhfs}`。
- `\kaishu`：楷书，CJK 等价命令 `\CJKfamily{zhkai}`。

参考：

- [中文宏包 CJK、xeCJK、luatexja、ctex 的区别和联系以及 UTF-8 编码的定义和在编译中的重要性 | CSDN](https://blog.csdn.net/weixin_45008608/article/details/115837858)
- [LaTeX 排版中文方案 | 知乎](https://zhuanlan.zhihu.com/p/647233033)
- [CTEX 宏集手册](https://ftp.kddilabs.jp/CTAN/language/chinese/ctex/ctex.pdf#page=5.63)