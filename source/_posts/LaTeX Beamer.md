LaTeX Beamer 就是 LaTeX 幻灯片。

Beamer 的基本结构：

```tex
% 设置文档类型为 beamer，宽高比为 16:9（默认为 4:3）
\documentclass[aspectratio=169]{beamer}

% 扉页（标题页）信息
\title{Sample title}
\author{Anonymous}
\institute{Overleaf}
\date{\today}

\begin{document}

% 创建扉页
\frame{\titlepage}

% 创建普通页
\begin{frame}{Sample frame title}
This is some text in the first frame.
\end{frame}

\end{document}
```

一个 frame 就是一个逻辑页

## minipage

基本用法：

```tex
\begin{minipage}[位置][高度][对齐]{宽度}
    % 这里是 minipage 内的内容
\end{minipage}
```

- 位置: 可选参数，指定 minipage 在垂直方向上的对齐方式。常见的值有 `t`（顶部对齐）、`c`（垂直居中）和 `b`（底部对齐）。
- 高度: 可选参数，指定 minipage 的高度。如果不指定，高度会自动适应内容。
- 对齐: 可选参数，指定内容在 minipage 内的垂直对齐方式。常见的值有 `t`、`c` 和 `b`。
- 宽度: 必须参数，指定 minipage 的宽度。

```tex
\begin{frame}
    \begin{minipage}[c]{0.45\textwidth}
        This is the left minipage.
    \end{minipage}
    \hfill
    \begin{minipage}[c]{0.45\textwidth}
        This is the right minipage.
    \end{minipage}
\end{frame}
```

- 参考：[THU Beamer Theme](https://www.overleaf.com/latex/templates/thu-beamer-theme/vwnqmzndvwyb)
- 参见：[Beamer | Overleaf](https://www.overleaf.com/learn/latex/Beamer)