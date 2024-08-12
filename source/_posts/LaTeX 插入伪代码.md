## 使用 algorithm 包和 algpseudocode 包

`algorithm` 包

- 用途: 提供一个浮动体环境来包含算法（类似于 figure 和 table 环境），使得算法可以自动编号并出现在文档中合适的位置。

- 功能:
   - 创建一个 algorithm 环境，用于包含算法内容。
   - 自动为算法编号并生成标题，方便在文档中引用。
   - 支持将算法放置在合适的位置（类似于图表的浮动机制）。

`algpseudocode` 包

- 用途: 提供伪代码的语法和命令，方便在 LaTeX 中书写算法的伪代码表示。

- 功能:
   - 提供一组命令来表示常见的伪代码结构，如 `\If`, `\While`, `\For`, `\State` 等。
   - 支持各种伪代码语法高亮和格式化，包括缩进、注释等。
   - 可以与 algorithm 包配合使用，集成到浮动体环境中。

```latex
% Preamble
\usepackage{algorithm}
\usepackage{algpseudocode}
```

```latex
% Body
\begin{algorithm}[htbp]
\caption{An algorithm with caption} % 设置标题
\label{alg:cap} % 设置引用标签
\begin{algorithmic} % 输入伪代码

\Require $n \geq 0$
\Ensure $y = x^n$
\State $y \gets 1$
\State $X \gets x$
\State $N \gets n$
\While{$N \neq 0$}
    \If{$N$ is even}
        \State $X \gets X \times X$
        \State $N \gets \frac{N}{2}$  \Comment{This is a comment}
    \ElsIf{$N$ is odd}
        \State $y \gets y \times X$
        \State $N \gets N - 1$
    \EndIf
\EndWhile

\end{algorithmic}
\end{algorithm}
```

![image](https://img2024.cnblogs.com/blog/2778973/202406/2778973-20240607215754716-1889219524.png)

参考：[Algorithms | Overleaf](https://www.overleaf.com/learn/latex/Algorithms)

### 修改注释符

`\Comment` 命令默认会创建一个右三角 `▷` 符号表示注释。如果你希望使用其他符号，可以在引入 `algpseudocode` 包后使用下面这条命令：

```tex
\renewcommand{\algorithmiccomment}[1]{\hfill // #1} % 将注释符改为 //
```
