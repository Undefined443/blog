### 介绍

`cleveref` 宏包是 LaTeX 中用于增强交叉引用功能的一个强大工具。它的主要特点是能够自动地按照不同元素的类型（如章节、图表等）生成格式化的引用，同时还支持定制引用格式，提供了比 LaTeX 默认的 `\ref` 更多的灵活性和功能。

传统的 `\ref` 命令只会输出标签对应的编号，而 `cleveref` 会在编号前自动添加对应的类型。例如，对于一个图像的标签，`cleveref` 可以自动生成“Figure 1”的形式，而不仅仅是“1”。

`cleveref` 也允许用户自定义各种元素的引用格式，包括单个引用和多个引用的情况。

### 基本用法

```latex
% Preamble
\usepackage{cleveref}
```

```latex
% Body
\section{xxx} \label{sec:xxx}  % 定义标签
...
\Cref{sec:xxx}  % 使用标签
```

生成结果为 `Section 2.8`

### 自定义引用格式

#### crefname

使用 crefname 命令，可以修改引用序号前面的文字。命令接受两个参数，第一个参数为引用单个对象时的类型文字，第二个参数为引用多个对象时的类型文字。

比如说，`\Crefname{figure}[Fig.][Figs.]` 可以让我们引用单个图片时的渲染结果为 `Fig. 1`，而引用多个图片时的渲染结果为 `Figs. 1-2`。

crefname 命令有大写版和小写版。你可以设置 `\crefname{figure}[fig.][figs.]`（注意这里的 `c` 为小写），这样你使用 `\cref{fig:image}` 时的渲染结果就是 `fig. 1` 或者 `figs. 1-2`。

下面是一些常用的 crefname 设置。

```latex
\crefname{table}{表}{表}
\Crefname{table}{表}{表}
\crefname{algorithm}{算法}{算法}
\Crefname{algorithm}{算法}{算法}
\Crefname{figure}{图}{图}
\crefname{figure}{图}{图}
\crefname{claim}{断言}{断言}
\Crefname{claim}{断言}{断言}
\crefname{lemma}{引理}{引理}
\Crefname{lemma}{引理}{引理}
```

#### crefformat

crefname 的局限性在于我们只能设置引用数字前面的类型文字。而 crefformat 则支持我们同时设置引用数字前面的文字和后面的文字。

`\crefformat` 用于指定单个引用的格式。命令的形式是：

```latex
\crefformat{<type>}{<format>}
```

- `<type>` 是引用的类型，如 `section`、`figure`、`table` 等。
- `<format>` 是当引用这种类型时使用的格式化字符串，它可能包含：
  - `#1`：引用标签
  - `#2`：前链接文本（默认为空）
  - `#3`：后链接文本（默认为空）

> 和 crefname 一样，crefformat 也有大写和小写两种命令

例：

```latex
% 对代码行引用格式的定义
\crefformat{line}{第~#2#1#3~行}
```

当我们引用一行代码时，`\Cref{alg:myalg:code}` 就会被渲染成 `第 1 行`。

#### crefrangeformat

`\crefmultiformat` 用于定义多个引用的格式。命令的形式是：

```latex
\crefmultiformat{<type>}{<format>}{<middle format>}{<between-last>}{<after-last>}
```

- `<type>` 是引用的类型。
- `<format>` 是对第一个引用使用的格式。
- `<middle format>` 是第一个和最后一个之间的其他引用使用的格式。
- `<between-last>` 是倒数第二个和最后一个引用之间的格式。
- `<after-last>` 是最后一个引用之后的格式。

例：

```latex
\crefmultiformat{section}{\S\S#2#1#3}{及~#2#1#3}{, #2#1#3}{, and~#2#1#3}
```

这将设置对多个节的引用格式。例如，引用 `\cref{sec:first,sec:second,sec:third}` 将产生 "§§1, 2, and 3"，其中第一个引用使用 "§§"，中间的引用用逗号隔开，最后一个引用前使用 "and" 作为连接符。

See also: [cleveref Doc](http://mirrors.ctan.org/macros/latex/contrib/cleveref/cleveref.pdf)