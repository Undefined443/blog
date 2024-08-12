编译包含交叉引用的 LaTeX 文件需要编译四次（pdflatex + bibtex + pdflatex * 2），一直对这四次编译都干了什么事很好奇。这次就来看一下每一步具体都干了些什么。

### 源文件

main.tex

```tex
\documentclass{article}
\begin{document}

Here is a citation \cite{example}.

\bibliographystyle{plain}
\bibliography{references}

\end{document}
```

references.bib

```bib
@article{example,
  author  = {Author Name},
  title   = {Title of the Paper},
  journal = {Journal Name},
  volume  = {XX},
  number  = {YY},
  pages   = {ZZ--ZZ},
  year    = {Year},
}
```

### 1. pdflatex

命令：

```sh
$ pdflatex main.tex
This is pdfTeX, Version 3.141592653-2.6-1.40.26 (TeX Live 2024) (preloaded format=pdflatex)
 restricted \write18 enabled.
entering extended mode
(./main.tex
LaTeX2e <2023-11-01> patch level 1
L3 programming layer <2024-02-20>
(/usr/local/texlive/2024/texmf-dist/tex/latex/base/article.cls
Document Class: article 2023/05/17 v1.4n Standard LaTeX document class
(/usr/local/texlive/2024/texmf-dist/tex/latex/base/size10.clo))
(/usr/local/texlive/2024/texmf-dist/tex/latex/l3backend/l3backend-pdftex.def)
No file main.aux.

LaTeX Warning: Citation `example' on page 1 undefined on input line 4.

No file main.bbl.
[1{/usr/local/texlive/2024/texmf-var/fonts/map/pdftex/updmap/pdftex.map}]
(./main.aux)

LaTeX Warning: There were undefined references.

 )</usr/local/texlive/2024/texmf-dist/fonts/type1/public/amsfonts/cm/cmbx10.pfb
></usr/local/texlive/2024/texmf-dist/fonts/type1/public/amsfonts/cm/cmr10.pfb>
Output written on main.pdf (1 page, 23198 bytes).
Transcript written on main.log.
```

输出：

main.aux

```tex
\relax
\citation{example}
\bibstyle{plain}
\bibdata{references}
\gdef \@abspage@last{1}
```

main.log（日志文件，略）

main.pdf

![image](https://img2024.cnblogs.com/blog/2778973/202405/2778973-20240522180734897-512862372.png)

> 可以看到目前只有交叉引用的占位符。

### 2. bibtex

命令：

```sh
$ bibtex main.aux
This is BibTeX, Version 0.99d (TeX Live 2024)
The top-level auxiliary file: main.aux
The style file: plain.bst
Database file #1: references.bib
```

输出：

main.bbl

```tex
\begin{thebibliography}{1}

\bibitem{example}
Author Name.
\newblock Title of the paper.
\newblock {\em Journal Name}, XX(YY):ZZ--ZZ, Year.

\end{thebibliography}
```

> `.bbl` 文件包含的实际上是 LaTeX 代码，我们可以直接将 `.bbl` 文件的内容插入到 `main.tex` 中去。只要用 `.bbl` 文件的内容替换 `\bibliography{}` 命令即可。

main.blg（日志文件，略）

### 3. pdflatex

命令：

```sh
$ pdflatex main.tex
This is pdfTeX, Version 3.141592653-2.6-1.40.26 (TeX Live 2024) (preloaded format=pdflatex)
 restricted \write18 enabled.
entering extended mode
(./main.tex
LaTeX2e <2023-11-01> patch level 1
L3 programming layer <2024-02-20>
(/usr/local/texlive/2024/texmf-dist/tex/latex/base/article.cls
Document Class: article 2023/05/17 v1.4n Standard LaTeX document class
(/usr/local/texlive/2024/texmf-dist/tex/latex/base/size10.clo))
(/usr/local/texlive/2024/texmf-dist/tex/latex/l3backend/l3backend-pdftex.def)
(./main.aux)

LaTeX Warning: Citation `example' on page 1 undefined on input line 4.

(./main.bbl) [1{/usr/local/texlive/2024/texmf-var/fonts/map/pdftex/updmap/pdfte
x.map}] (./main.aux)

LaTeX Warning: There were undefined references.


LaTeX Warning: Label(s) may have changed. Rerun to get cross-references right.

 )</usr/local/texlive/2024/texmf-dist/fonts/type1/public/amsfonts/cm/cmbx10.pfb
></usr/local/texlive/2024/texmf-dist/fonts/type1/public/amsfonts/cm/cmbx12.pfb>
</usr/local/texlive/2024/texmf-dist/fonts/type1/public/amsfonts/cm/cmr10.pfb></
usr/local/texlive/2024/texmf-dist/fonts/type1/public/amsfonts/cm/cmti10.pfb>
Output written on main.pdf (1 page, 46310 bytes).
Transcript written on main.log.
```

输出：

main.aux

```tex
\relax
\citation{example}
\bibstyle{plain}
\bibdata{references}
\bibcite{example}{1}
\gdef \@abspage@last{1}
```

> 可以看到这次 `.aux` 文件多了一条 `\bibcite{example}{1}` 命令。

main.log（日志文件，略）

main.pdf

![image](https://img2024.cnblogs.com/blog/2778973/202405/2778973-20240522192102092-904164000.png)

> 可以看到现在打印了参考文献列表，但是还没有引用到参考文献序号。

### 4. pdflatex

命令：

```sh
$ pdflatex main.tex
This is pdfTeX, Version 3.141592653-2.6-1.40.26 (TeX Live 2024) (preloaded format=pdflatex)
 restricted \write18 enabled.
entering extended mode
(./main.tex
LaTeX2e <2023-11-01> patch level 1
L3 programming layer <2024-02-20>
(/usr/local/texlive/2024/texmf-dist/tex/latex/base/article.cls
Document Class: article 2023/05/17 v1.4n Standard LaTeX document class
(/usr/local/texlive/2024/texmf-dist/tex/latex/base/size10.clo))
(/usr/local/texlive/2024/texmf-dist/tex/latex/l3backend/l3backend-pdftex.def)
(./main.aux) (./main.bbl) [1{/usr/local/texlive/2024/texmf-var/fonts/map/pdftex
/updmap/pdftex.map}] (./main.aux) )</usr/local/texlive/2024/texmf-dist/fonts/ty
pe1/public/amsfonts/cm/cmbx12.pfb></usr/local/texlive/2024/texmf-dist/fonts/typ
e1/public/amsfonts/cm/cmr10.pfb></usr/local/texlive/2024/texmf-dist/fonts/type1
/public/amsfonts/cm/cmti10.pfb>
Output written on main.pdf (1 page, 36642 bytes).
Transcript written on main.log.
```

输出：

main.log（日志文件，略）

main.pdf

![image](https://img2024.cnblogs.com/blog/2778973/202405/2778973-20240522192356039-215486853.png)

> 可以看到交叉引用已经生成完整。
