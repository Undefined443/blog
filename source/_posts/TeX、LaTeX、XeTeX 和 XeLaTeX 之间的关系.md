## TL;DR

总的来说，在 TeX 世界有两个主要概念，一个是 TeX，一个是 LaTeX。TeX 是一个排版引擎，它为用户提供的排版命令较为底层。LaTeX 是在 TeX 的基础上制作的宏包，它可以让用户不再关注那些底层命令。XeTeX 和 XeLaTeX 分别是 TeX 和 LaTeX 的 Unicode 版本，他们弥补了 TeX 和 LaTeX 只能编译英文文档的缺陷，其使用的命令与 TeX 和 LaTeX 完全相同。

## LaTeX 和 TeX

LaTeX 和 TeX 的关系可以用下面的方式描述：

1. TeX 是一种由 Donald Knuth 在 1978 年创建的排版系统，它提供了一套强大的宏语言及工具用于排版文档，特别是数学、物理学和计算机科学的论文。TeX 是底层的排版引擎，它允许用户通过编程式的方式精确控制文档的版面布局。
2. LaTeX 是一个构建在 TeX 之上的宏包，由 Leslie Lamport 在 20 世纪 80 年代初期开发。LaTeX 使用 TeX 作为其排版引擎，但为用户提供了更加方便、抽象化的接口来撰写和排版文档。LaTeX 通过预定义好的模板（称为类文件）和宏命令，简化了文档的排版流程，让用户更加专注于内容的编写而不是版面设计的细节。
3. 总结来说，TeX 是基础设施，提供了排版文档的基本机制和工具，而 LaTeX 是建构在这些设施之上的建筑，提供了易于使用的接口和功能。实际上，大多数使用 TeX 排版系统的用户都是通过 LaTeX 这一层来进行文档的编写和排版的，很少直接使用纯 TeX 命令进行文档排版。LaTeX 简化了 TeX 的复杂性，并扩展了它的功能，使其更加适合于撰写各类文档，从简单的文章到完整的书籍。

简而言之，LaTeX 是 TeX 的一个宏集，用户通常是与 LaTeX 交互，而 TeX 则在底层工作。

从 LaTeX 处理的代码和 TeX 处理的代码之间我们可以看出 LaTeX 和 TeX 的区别：

下面是一段简单的 TeX 代码示例：

```tex
\font\myfont=cmr12 at 12pt
\myfont
Hello, world!
\bye
```

上面的代码用 TeX 设置字体大小并打印 “Hello, world!”。

然后，这里是一段 LaTeX 代码示例：

```tex
\documentclass[12pt]{article}
\begin{document}
Hello, world!
\end{document}
```

可以看到 LaTeX 更专注于文档层面的格式调整，而 TeX 的调整则更底层。

这个 LaTeX 示例设置了一个基本的文档，包含文章类别和指定了 12pt 的字体大小，然后输出 “Hello, world!”。

这两个示例展示了 TeX 和 LaTeX 两种不同水平的使用。TeX 距离排版的低层操作更近，而 LaTeX 提供了更加用户友好的接口和更复杂的宏供用户使用。在实际应用中，LaTeX 比纯 TeX 更为常用，因为它大大简化了文档的组织。

如果要编译 TeX 文档，你可以使用 `tex` 工具将 `.tex` 文件编译为 `.dvi` 文件，或者使用 `pdftex` 工具将 `.tex` 文件编译为 PDF 文件。

如果要编译 LaTeX 文档，你可以使用 `latex` 工具将 `.tex` 文件编译为 `.dvi` 文件，或者使用 `pdflatex` 工具将 `.tex` 文件编译为 PDF 文件。

## XeTeX 和 TeX

XeLaTeX、XeTeX、和 TeX 之间的关系可以通过它们各自的用途和能力来理解：

- TeX：TeX 是由 Donald E. Knuth 开发的排版系统，它非常适合进行数学、科技和计算机文档的排版。TeX 系统提供了一系列工具用于文档排版，其中包含用于设置和调整字体、版面和间距的命令和环境。LaTeX 就是建立在 TeX 之上的一种排版系统，目的是简化文档的编写，它使用宏来自动化排版过程，让作者可以不用过多考虑文档的格式。
- XeTeX：XeTeX（“Zee-Tech”）是由 Jonathan Kew 开发的 TeX 排版引擎，并作为 TeX Live 和其他发行版的一部分提供。XeTeX 的显著特点在于其对现代字体技术的支持，如 Unicode、OpenType 和 TrueType，这使得它能更好地支持多语言排版和复杂的字体特性。XeTeX 可以直接访问系统中安装的字体，这是传统的 TeX 和 LaTeX 系统所不能做到的。
- XeLaTeX：XeLaTeX 实际上是在 XeTeX 之上使用 LaTeX 宏包的排版系统。它结合了 XeTeX 引擎的功能和 LaTeX 的易于使用性。XeLaTeX 使得用户能够通过 LaTeX 的语法来利用 XeTeX 的强大特性，如直接使用系统字体和复杂的文字排版能力。

总结起来，TeX 是整个系统的基础，LaTeX 是在 TeX 基础上提供高级宏的排版系统，XeTeX 是 TeX 的扩展，提供对现代字体和编码的支持，而 XeLaTeX 则将 LaTeX 的宏功能带入到 XeTeX 的世界，保留了 LaTeX 的易用性并且增加了对现代字体特性的支持。

简单地说，XeLaTeX 可以认为是 LaTeX 到 XeTeX 的过渡：它既能让你使用 LaTeX 的复杂宏来排版复杂的文档，同时又允许你利用 XeTeX 处理现代字体和编码系统的能力。