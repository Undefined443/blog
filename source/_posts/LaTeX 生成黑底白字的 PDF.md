最近需要深夜看论文，然而白底的 PDF 看久了眼睛很难受，想转换成黑底的。正好我有论文的 LaTeX 源码，因此可以直接编译黑底的 PDF 出来。

### 使用 darkmode 宏包

CTAN 上有一个 LaTeX 宏包 [darkmode](https://ctan.org/pkg/darkmode)，使用它可以编译出暗色背景的 PDF。

直接在导言区引入宏包并添加 enable 选项：

```tex
\usepackage[enable]{darkmode}
```

生成效果：

![image](https://img2024.cnblogs.com/blog/2778973/202405/2778973-20240524010530344-1323036067.png)

### 自定义样式文件

我们也可以自己配置一个样式文件，在需要改变页面颜色时引入这个样式文件就可以了：

样式文件：

```tex
% mydarkmode.sty - A package for dark background and bright text

\NeedsTeXFormat{LaTeX2e}
\ProvidesPackage{mydarkmode}[2024/05/24 Dark background with bright text]

\RequirePackage{pagecolor}
\RequirePackage{xcolor}

% Set the background color
\pagecolor{black}

% Set the text color
\color{white}
```

主文件：

```tex
\usepackage{mydarkmode}
```

生成效果：

![image](https://img2024.cnblogs.com/blog/2778973/202405/2778973-20240524015625549-1161929727.png)
