### 普通表格

```latex
\begin{table}[h]  % h: here
    \begin{center}
    % 一个字母代表一列
    \begin{tabular}{|c|cccc|}  % c: center, l: left, r: right
    \hline
      & 00 & 01 & 11 & 10\\
    \hline
    0 & $m_0$ & $m_1$ & $m_3$ & $m_2$\\
    1 & $m_4$ & $m_5$ & $m_7$ & $m_6$\\
    \hline
    \end{tabular}
    \end{center}
    \caption{表格说明}
    \label{tab:1}  % 引用标签
\end{table}
```

### 三线表

```latex
% Preamble
\usepackage{booktabs}
```

```latex
% Body
\begin{table}[h]
    % center 和 centering 的区别是 center 和上下文之间有垂直间隙
    % 而 centering 没有
    \centering
    \begin{tabular}{cccc}
    \toprule  % 第一道横线
    Item 1 & Item 2 & Item 3 & Item 4 \\
    \midrule  % 第二道横线 
    Data 1 & Data 2 & Data 3 & Data 4 \\
    Data 5 & Data 6 & Data 7 & Data 8 \\
    \bottomrule  % 第三道横线
    \end{tabular}
    \caption{表格说明}
    \label{tab:2}
\end{table}
```
