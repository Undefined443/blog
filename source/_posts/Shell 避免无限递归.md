在编写 Shell 脚本时，有时会产生我们不期望的递归。

比如说，我曾经写过一个脚本，名为 `foo.sh`。

`foo.sh` 的内容如下：

```sh
function foo {
  # TODO
}

foo
```

然后我在 `.zshrc` 里设置了别名：

```sh
alias foo="source ~/foo.sh"
```

现在，当我在终端运行 `foo` 时，就会得到如下错误：

```
/Users/undefined443/foo.sh:source:5: too many open files: /Users/undefined443/foo.sh
```

原因是 `foo.sh` 中的 `foo` 命令被当成别名，造成了无限递归。如果要临时回避这个别名，使用定义的 `foo` 函数，你可以在 `foo` 命令前加上 `\`，像这样：

```
\foo
```

这样做会忽略别名，直接运行 `foo` 函数。这在脚本编写中尤其有用，因为它允许脚本忽略用户定义的别名，确保脚本的行为是一致和预期的。
