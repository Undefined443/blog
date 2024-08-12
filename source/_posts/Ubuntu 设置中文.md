首先安装中文语言包：

```sh
sudo apt install -y language-pack-zh-hans
```

接下来在 `~/.zshrc` 或 `~/.bashrc` 中添加如下内容：

```sh
export \
    LANGUAGE="zh_CN.UTF-8" \
    LC_ALL="zh_CN.UTF-8" \
    LC_TIME="zh_CN.UTF-8" \
    LC_CTYPE="zh_CN.UTF-8" \
    LC_MONETARY="zh_CN.UTF-8" \
    LC_COLLATE="zh_CN.UTF-8" \
    LC_ADDRESS="zh_CN.UTF-8" \
    LC_TELEPHONE="zh_CN.UTF-8" \
    LC_MESSAGES="zh_CN.UTF-8" \
    LC_NAME="zh_CN.UTF-8" \
    LC_MEASUREMENT="zh_CN.UTF-8" \
    LC_IDENTIFICATION="zh_CN.UTF-8" \
    LC_NUMERIC="zh_CN.UTF-8" \
    LC_PAPER="zh_CN.UTF-8" \
    LANG="zh_CN.UTF-8"
```
