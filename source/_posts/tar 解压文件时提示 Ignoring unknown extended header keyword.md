在 Linux 上使用 tar 解压文件时出现下列提示：

```
tar: Ignoring unknown extended header keyword 'LIBARCHIVE.xattr.com.apple.quarantine'
tar: Ignoring unknown extended header keyword 'LIBARCHIVE.xattr.com.apple.provenance'
tar: Ignoring unknown extended header keyword 'LIBARCHIVE.xattr.com.apple.metadata:kMDItemWhereFroms'
tar: Ignoring unknown extended header keyword 'LIBARCHIVE.xattr.com.apple.macl'
```

当 tar 文件在 macOS 上创建，在 Linux 上提取时，就会发生这种情况。原因是 macOS 默认使用 BSD tar，而 Linux 默认使用 GNU tar。BSD tar 在创建归档文件的同时加入了一些 macOS 文件特有的扩展属性，如 `xattr.com.apple.quarantine`，这些信息不被 GNU tar 所识别。

实际上出现这些提示不必担心，提取过程是正常的。

如果你不希望产生这些提示，下次在 macOS 中使用 BSD tar 创建存档文件时，请使用 `--no-xattrs` 选项，这将关闭生成的存档文件中的 `xattr` 头部。

```sh
tar --no-xattrs -rvf <tar_file> <file>
```