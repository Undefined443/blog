LaTeX 本身并不直接支持导出 SVG 格式的文档或图片，但可以通过一些工具和插件实现将 LaTeX 文档或图形转换为 SVG 格式。

### 使用 dvisvgm

我们可以先将 LaTeX 文档编译为 DVI 格式，再通过 `dvisvgm` 工具将 DVI 文件转换为 SVG 格式。这个工具是 TeX Live 发行版的一部分。

1. 编译为 DVI 文件

   ```sh
   latexmk -dvi main.tex
   ```

2. 转换为 SVG

   ```sh
   dvisvgm main.dvi
   ```

### 使用 pdf2svg

如果你已经有一个 PDF 文件，可以使用 `pdf2svg` 工具将 PDF 文件转换为 SVG 文件。

```sh
pdf2svg main.pdf main.svg
```

### 使用 inkscape

Inkscape 是一个开源的矢量图形编辑器，它可以打开 PDF 文件并将其导出为 SVG 格式。

```sh
inkscape main.pdf --export-filename=main.svg
```
