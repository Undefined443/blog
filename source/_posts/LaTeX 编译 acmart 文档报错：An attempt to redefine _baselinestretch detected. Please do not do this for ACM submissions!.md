在编译一篇从 arXiv 下载的文档时遇到如下错误：

```
Class acmart Error: An attempt to redefine \baselinestretch detected. Please do not do this for ACM submissions!.
```

根据 StackExchange 问题 [How to make acmart stop complaining about \baselinestretch?](https://tex.stackexchange.com/questions/647338/how-to-make-acmart-stop-complaining-about-baselinestretch) 中的回答，错误的原因是你引入的包中有的修改了 `baselinestretch`（行间距）的值。而 ACM 要求 `baselinestretch` 必须为 1.25，否则生成的文档格式不符合投稿要求。

解决方式是在引入包之前保存当前的 `baselinestretch` 值，并在引入包之后恢复 `baselinestretch` 的值：

```latex
\let\savedbaselinestretch\baselinestretch  % 保存 baselinestretch 的值
\usepackage{oz}  % 一个会修改 baselinestretch 值的包
\let\baselinestretch\savedbaselinestretch  % 恢复 baselinestretch 的值
```
