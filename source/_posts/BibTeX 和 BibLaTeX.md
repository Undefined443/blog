- BibTeX：传统的参考文献处理工具，使用 `.bst` 文件来定义参考文献的样式。
- BibLaTeX：功能更强大且更现代的工具，使用 `.bbx`、`.cbx` 和 `.dbx` 文件来定义参考文献和引用的样式。

BibTeX 使用 `.bst` 文件来指定参考文献列表的格式。

使用方法：

```tex
\documentclass{article}

% 指定自定义的 .bst 文件
\bibliographystyle{custom}

\begin{document}

这是一个引用示例 \cite{sample2023}。

% 打印参考文献列表
\bibliography{references}

\end{document}
```

BibLaTeX 使用 `.bbx`、`.cbx` 和 `.dbx` 文件来控制引用格式。具体来说：

- `.bbx` 文件定义了参考文献列表的样式。
- `.cbx` 文件定义了文中引用的样式。
- `.dbx` 文件定义了数据模型。

使用方法：

```tex
\documentclass{article}
\usepackage[backend=biber,style=authoryear]{biblatex}

% 加载自定义的 .bbx, .cbx 和 .dbx 文件
\DeclareBibliographyStyleFile{custom}{custom.bbx}
\DeclareCitationStyleFile{custom}{custom.cbx}
\DeclareDatamodelFile{custom}{custom.dbx}

\addbibresource{references.bib}

\begin{document}

这是一个引用示例 \cite{sample2023}。

\printbibliography

\end{document}
```