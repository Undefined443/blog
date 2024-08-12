在 DOS/Windows 文本文件中，换行，也称为新行，是两个字符的组合：回车（CR）后跟一个换行（LF）。在 Unix 文本文件中，一行的换行是单个字符：换行（LF）。在 Mac 文本文件中，在 Mac OS X 之前，一行的换行是单个回车（CR）字符。现在的 Mac OS 使用 Unix 风格（LF）的换行。

在 Unix-like 系统上使用换行符为 CRLF 的文件可能会带来不便。

比如说，我将换行符为 CRLF 的 vim 配置文件用于我的 macOS 上的 vim，结果打开 vim 时会出现这样的错误提示：

```
Error detected while processing /Users/user/.vimrc[14]../Users/user/.vim/pack/themes/start/dracula_pro/colors/dracula_pro.vim:
line    2:
E492: Not an editor command: ^M
```

在这种情况下，错误的来源是 `^M` 字符，这是一个回车符（Carriage Return, CR）。在 Windows 系统中，行尾标记通常为 CRLF（即 `\r\n`），而在 Unix 和 Linux 系统，包括 macOS 在内，行尾标记只有一个 LF（即 `\n`）。

因此，你需要将文件的换行符由 CRLF 转换为 LF。

## 解决方法

### CRLF 转 LF

1. 如果你需要转换的文件有很多，你可以借助 git 的自动转换换行符功能：

   ```sh
   git init  # 建立存储库
   git config core.autocrlf input  # 设置提交到存储库时转换为 LF
   git add . && git commit -m "tmp"  # 提交所有文件
   git rm --cached -r .  # 删除 git 中的缓存文件
   git reset --hard  # 重置项目文件
   ```

   现在所有包含 CRLF 的文件都将被转换为 LF。

   参考：[Converting the End of Line Sequence from CRLF to LF in any of your project files | GitHub](https://gist.github.com/tjsudarsan/f2bfe4c4d567243e302cf8ba40e1c7e5)

2. 使用 `dos2unix` 工具：

   - 转换单个文件：

      ```sh
      dos2unix <CRLF_file>  # 将 `CRLF_file` 替换为你要转换的文件名
      ```

   - 转换目录下的所有文件

      ```sh
      find . -type f -print0 | xargs -0 dos2unix --
      ```

   参考：[How to apply dos2unix recursively to all the contents of a folder? | Stack Exchange](https://unix.stackexchange.com/questions/279813/how-to-apply-dos2unix-recursively-to-all-the-contents-of-a-folder)

### LF 转 CRLF

1. 借助 git 转换整个项目内的文件

   ```sh
   git init  # 建立存储库
   git config core.autocrlf true  # 设置提交到存储库时转换为 LF，从存储库取回时转换为 CRLF
   git add . && git commit -m "tmp"  # 提交所有文件
   git rm --cached -r .  # 删除 git 中的缓存文件
   git reset --hard  # 重置项目文件
   ```

2. 使用 `unix2dos` 工具：

   - 转换单个文件：

      ```sh
      unix2dos <CRLF_file>  # 将 `CRLF_file` 替换为你要转换的文件名
      ```

   - 转换目录下的所有文件

      ```sh
      find . -type f -print0 | xargs -0 unix2dos --
      ```

   参考：[批量轉換 LF 和 CRLF 的小技巧【詳細步驟】| 台部落](https://www.twblogs.net/a/5cb2636fbd9eee480f078e56)

### 附录：关于 autocrlf 配置的说明：

- `true`：提交到存储库时使用 LF，检出时使用 CRLF；
- `input`：提交到存储库时使用 LF，检出时不转换（还是 LF）；
- `false`：~~不进行任何转换（如果原来是 CRLF，那么提交到存储库时还是 CRLF，检出时也还是 CRLF）~~（并不是这样，在 macOS 上操作时效果感觉和 input 一样）

[git core.autocrlf 配置说明](https://www.cnblogs.com/youpeng/p/11243871.html)