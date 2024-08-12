在编写 Linux 的 `.service` 文件的时候，我发现 `.service` 文件的本质是 [INI 文件](https://zh.wikipedia.org/wiki/INI文件)。然而 VS Code 却并没有使用 INI 格式进行语法高亮。于是我通过如下设置使 VS Code 在遇到 `.service` 文件时自动使用 INI 格式的语法高亮：

打开设置，搜索：`files.associations`，并添加一个项目：

|Item|Value|
|:--|:--|
|*.service|ini|

如图：

![image](https://s2.loli.net/2024/07/07/5zdhkBP7TaGfV2R.png)

这样，VS Code 就会自动使用 INI 格式来对 `.service` 文件进行语法高亮了。

![image](https://s2.loli.net/2024/07/07/UN1mZYS9IzXqROw.png)
