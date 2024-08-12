```latex
% Preamble
\usepackage{graphicx}
```

```latex
% Body
\begin{figure}[ht]  % h: here, t: top
    \centering  % 居中
    \includegraphics[width=0.85\columnwidth]{figures/figure.pdf}  % 插入图片
    \caption{图片说明}
    \label{fig:1}  % 引用标签
\end{figure}
```

```latex
\ref{fig:1}  % 引用图片
```

参考：[Inserting Images | Overleaf](https://www.overleaf.com/learn/latex/Inserting_Images)