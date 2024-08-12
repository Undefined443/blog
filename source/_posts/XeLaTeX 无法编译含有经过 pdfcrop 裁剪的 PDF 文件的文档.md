今天在写 LaTeX 文档时踩了个大坑，我在文档里插入了一个 PDF 图片之后文档无法编译了。

于是我去掉多余代码，做了一个最小工作示例：

```latex
\documentclass{article}
\usepackage{graphicx}
\begin{document}

\includegraphics{my_image.pdf}  % 插入一张 pdfcrop 裁剪过的图片

\end{document}
```

就是这样一个简单的代码，pdfLaTeX 可以编译，XeLaTeX 却会报错。并且没有任何详细错误信息。

![image](https://img2024.cnblogs.com/blog/2778973/202405/2778973-20240514143703617-2082378713.png)

而且，这种情况只有在 Overleaf（包括本地部署的 Overleaf） 上编译时才会发生。我在本地用 `latexmk -xelatex` 命令编译是可以正常通过的。

问题的根源尚不清楚。经过我的一些测试，有如下现象：

- 只有在 Overleaf 上才有可能遇到这种错误；
- 不是所有 pdfcrop 裁剪的 PDF 图片都会报错；
- 如果你在最小示例中添加了一些可以正常编译的图片，那么这些正常的图片和异常的图片只有在特定的添加顺序下才能被正常编译。

问题很复杂。但目前我已找到一种解决方案。在使用 pdfcrop 裁剪 PDF 文件时，添加 `-xetex` 选项，这样 XeLaTeX 就能正确处理了：

```sh
pdfcrop -xetex input.pdf output.pdf
```