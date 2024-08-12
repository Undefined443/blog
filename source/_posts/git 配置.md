### 邮箱和姓名

```sh
git config --global user.name "username"
git config --global user.email "email@example.com"
```

### 默认编辑器

```sh
git config --global core.editor "vim"
```

### 远程库

```sh
git remote remove origin
git remote add origin [new-repository-url]
```

```sh
git push --set-upstream origin main
```