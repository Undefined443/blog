由于 frps/frpc 不是 Windows 服务应用程序，因此我们不能直接使用 `New-Service` 命令创建 frps/frpc 服务。我们可以使用下面的方法将 frps/frpc 封装为 Windows 服务应用程序，然后再使用 `Xxx-Service` 命令进行管理。

### 使用 WinSW

[WinSW](https://github.com/winsw/winsw) 是一个可以将任何应用程序封装并管理为 Windows 服务的程序。

#### 封装服务

首先下载 WinSW 程序：

打开 [WinSW Releases Page](https://github.com/winsw/winsw/releases/latest/)，选择一个适合你的可执行程序版本。

- x64: 不使用 .NET 框架的可执行程序，体积较大。
- NET461: 使用 .NET 框架的可执行程序，体积较小。461 是 .NET 框架版本号 4.6.1。

我这里选择使用 `WinSW.NET461.exe`。

1. 将你下载到的 WinSW 可执行程序放到你要封装的程序所在的目录下。
2. 创建和 WinSW 可执行程序同名的 XML 配置文件：

   - 比如，我下载的是 `WinSW-net461.exe`，那么我的配置文件名为 `WinSW-net461.xml`。
   - 在配置文件中填入如下内容：

      ```xml
      <service>
          <id>frpc</id>
          <name>frpc</name>
          <description>frpc</description>
          <executable>frpc</executable>
          <arguments>-c frpc.toml</arguments>
          <logmode>reset</logmode>
      </service>
      ```

3. 封装并运行 Windows 服务：

   ```powershell
   .\WinSW-net461.exe install  # 封装服务
   .\WinSW-net461.exe start  # 运行服务
   ```

可以使用 `winsw status` 命令查看服务的状态：

```powershell
$ .\WinSW-net461.exe status
Active (running)
```

显示 `Active (running)` 则证明我们的服务已经开始运行。

#### 删除服务

```powershell
./WinSW-net461.exe uninstall
```

参考：[Frp 内网穿透 Windows 设置开机启动的两种方法](https://hilau.com/1720/)