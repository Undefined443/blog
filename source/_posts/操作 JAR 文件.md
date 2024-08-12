### 列出 JAR 文件内容

使用 `jar` 命令来列出 JAR 文件的内容：

```sh
jar tf myapp.jar
```

`-t` 选项表示列出文件，`-f` 表示指定 JAR 文件。

### 解压 JAR 文件

使用 `jar` 命令的 `-x` 选项：

```sh
jar xf myapp.jar
```

这会将 JAR 文件的内容解压到当前目录。