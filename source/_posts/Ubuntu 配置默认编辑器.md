在 Ubuntu 系统中，可以通过多种方式配置默认的文本编辑器，以便在使用命令行工具（如 `crontab` 或 `visudo`）时自动使用你喜欢的编辑器。以下是几种常见的方法：

### 使用 `update-alternatives`

Ubuntu 提供了 `update-alternatives` 工具来管理系统中多个可替代的命令。你可以用它来设置默认的编辑器。

1. **查看当前的默认编辑器**:

   ```sh
   update-alternatives --display editor
   ```

2. **配置默认编辑器**:

   ```sh
   sudo update-alternatives --config editor
   ```

   这条命令会列出系统中所有可用的文本编辑器，并让你选择一个作为默认编辑器。你只需要输入所选编辑器对应的编号即可。

### 使用环境变量 `EDITOR` 和 `VISUAL`

你也可以通过设置环境变量 `EDITOR` 和 `VISUAL` 来指定默认的编辑器。

1. **编辑 Shell 配置文件**:

   根据你使用的 Shell 编辑相应的配置文件（例如 `.bashrc`、`.zshrc` 或 `.profile`）。

   ```sh
   nano ~/.bashrc
   ```

2. **添加环境变量**:
   在配置文件中添加以下行，以设置默认编辑器为 `nano`（你可以将 `nano` 替换为你喜欢的编辑器，如 `vim` 或 `emacs`）。

   ```sh
   export EDITOR=nano  # 非交互式
   export VISUAL=nano  # 交互式
   ```

3. **使配置生效**:
   保存文件并运行以下命令以使新配置生效：

   ```sh
   source ~/.bashrc
   ```

### 通过 `select-editor` 命令

Ubuntu 还提供了 `select-editor` 命令，可以快速且简单地设置默认编辑器。

1. **运行 `select-editor` 命令**:

   ```sh
   select-editor
   ```

2. **选择编辑器**:
   命令会列出系统中可用的编辑器，你只需输入对应的编号即可。