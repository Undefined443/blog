在 Windows 环境下进行绑定挂载时，需要注意路径的写法，需要使用 Windows 风格 (`C:\xxx\xxx`) 的路径，而不是 Cygwin (`/c/xxx/xxx`) 风格的路径。这一点在使用 Git Bash 时要尤为注意。

比如说，在 Git Bash 中，`pwd` 命令的输出是 `/c/Users/username/...`，而不是 `C:\Users\username\...`。因此如果使用了如下命令：

```sh
docker run -v "${PWD}":/app img
```

在 Git Bash 中可能会产生意料之外的绑定挂载。

另外，在 PowerShell 中进行绑定时出现过另一种奇怪现象，在使用上面的命令挂载时，可能会出现奇怪的报错，这时将命令改成如下（改变双引号的位置）：

```powershell
docker run -v "${PWD}:/app" myimage
```

就能正常运行。

这样的命令也会出现报错：

```powershell
$ docker run -v $(pwd)\frpc.ini:/etc/frp/frpc.ini frp
docker: 目录格式不正确
```

然而如果把 `$(pwd)` 换成 `$PWD` 就不会报错。两条命令 `echo` 出来的内容却是一样的。

这样的命令也不会报错。

```sh
docker run -d --network host --name frpc -v .\frpc.toml:/etc/frp/frpc.toml snowdreamtech/frpc
```