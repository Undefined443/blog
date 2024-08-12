在编译一篇中文文档时遇到如下警告：

```
Package fontspec Warning: Script 'CJK' not explicitly supported within font 'FandolSong-Regular'. Check the typeset output, and if it is okay then ignore this warning. Otherwise a different font should be chosen.
```

产生警告的原因有两个：

1. xeCJK 为每个 CJK 字体默认使用特性 `Script=CJK`
2. 部分 CJK 字体没有声明对应的特性，包括 xeCJK 默认使用的 Fandol 系列字体。

解决方案有两个：

1. 使用其他字体

   其实我个人更喜欢使用方正字体。方正字体是声明过 `Script=CJK` 特性的。要使用方正字体，在引入 `ctex` 包时添加配置选项就可以了：

   ```latex
   \usepackage[fontset=founder]{ctex}
   ```

2. 抑制警告的输出。见参考。

参考：[[LaTeX 中文使用] 抑制 xeCJK/fontspec 警告 Script 'CJK' not explicitly supported within font](https://zhuanlan.zhihu.com/p/145429470)