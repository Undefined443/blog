LaTeX 插入代码可以使用的宏包有 verbatim、fancyvrb、listings 以及 minted。个人最推荐使用 minted。

### [verbatim](https://ctan.org/pkg/verbatim)

verbatim 没有语法高亮功能，只是显示一个等宽字体的输出。[查看 Overleaf 示例](https://www.overleaf.com/docs?engine=pdflatex&snip_name=verbatim+text+example&snip=%5Cdocumentclass%7Barticle%7D%0A%5Cbegin%7Bdocument%7D%0A%5Cbegin%7Bverbatim%7D%0AText+enclosed+inside+%5Ctexttt%7Bverbatim%7D+environment+%0Ais+printed+directly+%0Aand+all+%5CLaTeX%7B%7D+commands+are+ignored.%0A%5Cend%7Bverbatim%7D%0A%5Cend%7Bdocument%7D)

```latex
% Preamble
\usepackage{verbatim}
```

```latex
% Body
\begin{verbatim}
Text enclosed inside \texttt{verbatim} environment 
is printed directly
and all \LaTeX{} commands are ignored.
\end{verbatim}
```

### [fancyvrb](https://ctan.org/pkg/fancyvrb)

fancyvrb 是 verbatim 的增强版，增加了显示行号和代码块边框的功能。

```latex
% Preamble
\usepackage{fancyvrb}
\usepackage{xcolor}  % 用到了 \color 命令
```

```latex
% Body
% 设置在代码块左部显示行号，用方框包围代码块，代码块显示为红色
\begin{Verbatim}[numbers=left, frame=single, formatcom=\color{black}]
#include <iostream>

int main() {
  std::cout << "Hello, world!" << std::endl;
  return 0;
}
\end{Verbatim}
```

### [listings](https://ctan.org/pkg/listings)

listings 则更为强大，带有语法高亮、自定义风格等功能。[查看 Overleaf 示例](https://www.overleaf.com/docs?engine=pdflatex&snip_name=listings+package+example&snip=%5Cdocumentclass%7Barticle%7D%0A%5Cusepackage%7Blistings%7D%0A%5Cbegin%7Bdocument%7D%0A%5Cbegin%7Blstlisting%7D%0Aimport+numpy+as+np%0A++++%0Adef+incmatrix%28genl1%2Cgenl2%29%3A%0A++++m+%3D+len%28genl1%29%0A++++n+%3D+len%28genl2%29%0A++++M+%3D+None+%23to+become+the+incidence+matrix%0A++++VT+%3D+np.zeros%28%28n%2Am%2C1%29%2C+int%29++%23dummy+variable%0A++++%0A++++%23compute+the+bitwise+xor+matrix%0A++++M1+%3D+bitxormatrix%28genl1%29%0A++++M2+%3D+np.triu%28bitxormatrix%28genl2%29%2C1%29+%0A%0A++++for+i+in+range%28m-1%29%3A%0A++++++++for+j+in+range%28i%2B1%2C+m%29%3A%0A++++++++++++%5Br%2Cc%5D+%3D+np.where%28M2+%3D%3D+M1%5Bi%2Cj%5D%29%0A++++++++++++for+k+in+range%28len%28r%29%29%3A%0A++++++++++++++++VT%5B%28i%29%2An+%2B+r%5Bk%5D%5D+%3D+1%3B%0A++++++++++++++++VT%5B%28i%29%2An+%2B+c%5Bk%5D%5D+%3D+1%3B%0A++++++++++++++++VT%5B%28j%29%2An+%2B+r%5Bk%5D%5D+%3D+1%3B%0A++++++++++++++++VT%5B%28j%29%2An+%2B+c%5Bk%5D%5D+%3D+1%3B%0A++++++++++++++++%0A++++++++++++++++if+M+is+None%3A%0A++++++++++++++++++++M+%3D+np.copy%28VT%29%0A++++++++++++++++else%3A%0A++++++++++++++++++++M+%3D+np.concatenate%28%28M%2C+VT%29%2C+1%29%0A++++++++++++++++%0A++++++++++++++++VT+%3D+np.zeros%28%28n%2Am%2C1%29%2C+int%29%0A++++%0A++++return+M%0A%5Cend%7Blstlisting%7D%0A%5Cend%7Bdocument%7D)

```latex
% Preamble
\usepackage{xcolor}  % 为了渲染颜色，需要使用 xcolor 包
\usepackage{listings}  % 渲染代码块
```

```latex
% Body

% 定义颜色
\definecolor{codegreen}{rgb}{0,0.6,0}
\definecolor{codegray}{rgb}{0.5,0.5,0.5}
\definecolor{codepurple}{rgb}{0.58,0,0.82}
\definecolor{backcolour}{rgb}{0.95,0.95,0.92}

% 定义 listings 风格，可以定义多个
\lstdefinestyle{mystyle}{
    % backgroundcolor=\color{backcolour},  % 背景色
    commentstyle= \color{red!50!green!50!blue!50},  % 注释的颜色
    keywordstyle= \color{blue!70},  % 关键字/程序语言中的保留字颜色
    numberstyle=\tiny\color{codegray},  % 左侧行号显示的颜色
    stringstyle=\color{codepurple},
    basicstyle=\ttfamily\footnotesize,
    breakatwhitespace=false,
    breaklines=true,  % 对过长的代码自动换行
    captionpos=b,
    keepspaces=true,
    numbers=left,  % 在左侧显示行号
    numbersep=5pt,
    showspaces=false,
    showstringspaces=false,  % 不显示字符串中的空格
    showtabs=false,
    tabsize=2,
    frame=single  % [none | single | shadowbox] 显示边框
}

\lstset{style=mystyle}  % 使用 listings 风格

\begin{lstlisting}[language=Python]
def main():
    print("Hello, world!")
    return 0
\end{lstlisting}
```

参考：[Code listing | Overleaf](https://www.overleaf.com/learn/latex/Code_listing)

### [minted](https://ctan.org/pkg/minted?lang=en)（推荐）

minted 是最方便好用的一款代码格式化宏包，开箱即用，只需配置目标语言即可。不过它需要 Python Pygments 包的依赖，以及需要启用 `shell-escape` 选项。[查看 Overleaf 示例](https://www.overleaf.com/docs?engine=pdflatex&snip_name=Example+of+the+minted+package&snip=%5Cdocumentclass%7Barticle%7D%0A%5Cusepackage%7Bminted%7D%0A%5Cbegin%7Bdocument%7D%0A%5Cbegin%7Bminted%7D%7Bpython%7D%0Aimport+numpy+as+np%0A++++%0Adef+incmatrix%28genl1%2Cgenl2%29%3A%0A++++m+%3D+len%28genl1%29%0A++++n+%3D+len%28genl2%29%0A++++M+%3D+None+%23to+become+the+incidence+matrix%0A++++VT+%3D+np.zeros%28%28n%2Am%2C1%29%2C+int%29++%23dummy+variable%0A++++%0A++++%23compute+the+bitwise+xor+matrix%0A++++M1+%3D+bitxormatrix%28genl1%29%0A++++M2+%3D+np.triu%28bitxormatrix%28genl2%29%2C1%29+%0A%0A++++for+i+in+range%28m-1%29%3A%0A++++++++for+j+in+range%28i%2B1%2C+m%29%3A%0A++++++++++++%5Br%2Cc%5D+%3D+np.where%28M2+%3D%3D+M1%5Bi%2Cj%5D%29%0A++++++++++++for+k+in+range%28len%28r%29%29%3A%0A++++++++++++++++VT%5B%28i%29%2An+%2B+r%5Bk%5D%5D+%3D+1%3B%0A++++++++++++++++VT%5B%28i%29%2An+%2B+c%5Bk%5D%5D+%3D+1%3B%0A++++++++++++++++VT%5B%28j%29%2An+%2B+r%5Bk%5D%5D+%3D+1%3B%0A++++++++++++++++VT%5B%28j%29%2An+%2B+c%5Bk%5D%5D+%3D+1%3B%0A++++++++++++++++%0A++++++++++++++++if+M+is+None%3A%0A++++++++++++++++++++M+%3D+np.copy%28VT%29%0A++++++++++++++++else%3A%0A++++++++++++++++++++M+%3D+np.concatenate%28%28M%2C+VT%29%2C+1%29%0A++++++++++++++++%0A++++++++++++++++VT+%3D+np.zeros%28%28n%2Am%2C1%29%2C+int%29%0A++++%0A++++return+M%0A%5Cend%7Bminted%7D%0A%5Cend%7Bdocument%7D)

安装 Pygments 包。以下命令选其一。

```sh
sudo apt install install python3-pygments
pip install pygments
```

启用 `shell-escape` 选项：

```sh
echo "shell_escape = t" >> /usr/local/texlive/2024/texmf.cnf

# 或者，在每次编译的时候指定 -shell-escape 选项
latexmk -shell-escape main.tex
```

> 注意将 `2024` 改为你当前在使用的 TeX Live 版本

如果你的编译套件不是 TeX Live，那么开启 `shell-escape` 选项的方法可以参考 [How can I enable shell-escape? | StackExchange](https://tex.stackexchange.com/questions/598818/how-can-i-enable-shell-escape)

```latex
% Preamble
\usepackage[outputdir=build]{minted}
```

```latex
% Body
\usemintedstyle{vs}  % 切换风格

\begin{minted}{python}
def main():
    print("Hello, world!")
    return 0
\end{minted}
```

> 如果你在编译时指定了特定的输出目录，才需要在引入 `minted` 包时提供 `outputdir` 选项。这里指定的输出目录是 `build`。

参考：[Code Highlighting with minted | Overleaf](https://www.overleaf.com/learn/latex/Code_Highlighting_with_minted)