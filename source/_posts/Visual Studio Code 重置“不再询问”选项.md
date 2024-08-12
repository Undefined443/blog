有一次使用 VS Code 重命名一个 Python 文件时，VS Code 询问“扩展‘Python’希望通过移动此文件来进行重构更改”。当时没有多想，选中“不再提问”之后就点了“确定”。结果后来后悔，觉得在进行更改之前应该先检查一下更改的内容。于是想要重置这个“不再询问”的选项。结果始终没有找到重置的位置。

![image](https://s2.loli.net/2024/07/07/1DBkm9b23Zzuvey.png)

在网上找了一些资料，看样子这种“不再询问”的设置可能保存在 `~/Library/Application Support/Code/User/workspaceStorage` 文件夹下（具体看这篇文章 [Allow to reset “Don't Show Again” preference](https://github.com/microsoft/vscode/issues/24815#issuecomment-547733206)）。但是我找了几个数据库文件都没找到相关设置。

最后一怒之下删除了 `~/Library/Application Support/Code` 文件夹。

于是就好了。