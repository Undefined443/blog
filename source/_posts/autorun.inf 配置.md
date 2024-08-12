`autorun.inf` 文件是一个配置文件，通常用于可移动磁盘（如 USB 驱动器和 CD/DVD）来自动执行某些操作或配置一些设置。当插入可移动磁盘时，Windows 会读取 `autorun.inf` 文件中的指令并执行相应的操作。以下是一些 `autorun.inf` 文件可以实现的功能：

1. **自动运行程序**：

   `autorun.inf` 文件可以配置在插入磁盘时自动运行特定的程序。例如：

   ```ini
   [autorun]
   open=setup.exe
   ```

   这将会自动运行根目录下的 `setup.exe` 程序。

2. **设置磁盘的图标**：

   设置磁盘在资源管理器中的显示图标：

   ```ini
   [autorun]
   icon=myicon.ico
   ```

3. **设置磁盘的标签**：

   可以为磁盘设置一个自定义标签：

   ```ini
   [autorun]
   label=My Custom Label
   ```

4. **添加右键菜单项**：

   可以为磁盘添加自定义的右键菜单项：

   ```ini
   [autorun]
   shell\readme=&Read Me
   shell\readme\command=notepad.exe README.txt
   ```

   这将添加一个名为 "Read Me" 的右键菜单项，点击时会打开记事本并显示 `README.txt` 文件。

5. **指定默认右键菜单项**：

   可以设置一个默认的右键菜单项，当用户双击磁盘图标时执行：

   ```ini
   [autorun]
   shell=readme
   shell\readme=&Read Me
   shell\readme\command=notepad.exe README.txt
   ```

6. **设置多种图标和标签（用于多卷）**：

   对于多卷集，可以为不同的卷设置不同的图标和标签：

   ```ini
   [autorun]
   icon=icon1.ico
   label=Volume 1

   [autorun1]
   icon=icon2.ico
   label=Volume 2
   ```

需要注意的是，现代版本的 Windows 对 `autorun.inf` 文件的功能有一定限制，特别是为了防止传播恶意软件。例如，自动运行（`open` 指令）通常在插入 USB 驱动器时被禁用，但在插入 CD/DVD 时仍可能有效。

参见：

- [Autorun.inf Entries | Microsoft Learn](https://learn.microsoft.com/en-us/windows/win32/shell/autorun-cmds)

- [autorun.inf | Wikipedia](https://en.wikipedia.org/wiki/Autorun.inf)