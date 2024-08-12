在编译一篇从 arXiv 下载的文档时遇到如下错误：

```
Package hyperxmp Error: hyperref must be loaded before hyperxmp.
```

根据 GitHub 上这个问题 [Load hyperref before hyperxmp ?](https://github.com/borisveytsman/acmart/issues/505) 的答案得到错误的原因是 `hyperxmp` 包的作者进行了一些更新，要求 `hyperref` 包必须在`hyperxmp` 包之前引入。而 `acmart` 文档类还未对此更新进行调整，并且其中的 `hyperref` 是在 `hyperxmp` 之后调用的。

解决方法就是我们手动修改 `acmart.cls` 文件，将其中的：

```latex
\RequirePackage{hyperxmp}
\let\@footnotemark@nolink\@footnotemark
\let\@footnotetext@nolink\@footnotetext
\RequirePackage[bookmarksnumbered,unicode]{hyperref}
```

修改为：

```latex
\let\@footnotemark@nolink\@footnotemark
\let\@footnotetext@nolink\@footnotetext
\RequirePackage[bookmarksnumbered,unicode]{hyperref}
\RequirePackage{hyperxmp}
```

也就是说，只要将 `\RequirePackage{hyperxmp}` 这行命令移动到 `\RequirePackage[bookmarksnumbered,unicode]{hyperref}` 的下面就行了。