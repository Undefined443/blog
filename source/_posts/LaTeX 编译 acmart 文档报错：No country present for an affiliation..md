在编译一篇从 arXiv 下载的文档时遇到如下错误：

```
Class acmart Error: No country present for an affiliation.
```

有两种解决方案：

1. 将错误降级为警告

   在 `\documentclass[xxx]{acmart}` 后添加如下代码：

   ```latex
   \makeatletter
   \def\@ACM@checkaffil{% Only warnings
       \if@ACM@instpresent\else
       \ClassWarningNoLine{\@classname}{No institution present for an affiliation}%
       \fi
       \if@ACM@citypresent\else
       \ClassWarningNoLine{\@classname}{No city present for an affiliation}%
       \fi
       \if@ACM@countrypresent\else
           \ClassWarningNoLine{\@classname}{No country present for an affiliation}%
       \fi
   }
   \makeatother
   ```

2. 为每个作者添加空的 `country` 信息

   在每个作者的 `\affiliation` 代码块中添加 `\country{}`

   ```latex
   \author{Xxx Xxx}
   \affiliation{
       \institution{xxx}
       \country{}  % 添加这行
   }
   ```

参考：[How to make acmart stop complaining about missing country in affiliation? | StackExchange](https://tex.stackexchange.com/questions/655620/how-to-make-acmart-stop-complaining-about-missing-country-in-affiliation)