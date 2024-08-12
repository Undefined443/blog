1. 进入交互式 rebase 模式

   ```sh
   git rebase -i <commit>
   ```

   你要修改哪次提交的日期，就 rebase 到该提交的上一次提交。

2. git 提示你新的分支要包含哪些提交，默认已经加载了你 rebase 的提交后面的所有提交。

   将你要修改日期的提交前面的选项改为 `edit`：

   ```
   edit abcdef1 First commit
   edit abcdef2 Second commit
   pick abcdef3 Third commit
   ```

3. 接下来会按顺序进入你要编辑的提交。此时我们可以修改提交。

   ```sh
   git commit --amend --reset-author
   ```

   `--reset-author` 选项会同时修改 author 和 author date

4. 完成编辑

   ```sh
   git rebase --continue
   ```

   如果你已经完成了所有要编辑的提交，在执行这条命令之后就完成了变基操作。如果还有未编辑的提交，则会进入下一个提交。

5. 如果你要上传到远程仓库，使用 `--force` 选项。

   ```sh
   git push –force
   ```

   注意这会覆盖仓库中的原有内容。
