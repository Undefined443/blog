[Rust 官网](https://www.rust-lang.org/zh-CN/learn/get-started)

## 安装

### 使用包管理器

Homebrew:

```sh
brew install rust
echo 'export PATH="/home/xiao/.cargo/bin:$PATH"' >> ~/.zshrc
```

APT:

```sh
sudo apt install cargo
echo 'export PATH="/home/xiao/.cargo/bin:$PATH"' >> ~/.zshrc
```

### 使用官方脚本

macOS / Linux:

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

卸载：

```sh
rustup self uninstall
```

参考：[如何在 Ubuntu 和其它的 Linux 发行版安装 Rust 和 Cargo | Linux 中国](https://linux.cn/article-13938-1.html)

## 使用

Cargo 是 Rust 的构建工具和包管理器。

官方 Cargo 教程：[The Cargo Book](https://doc.rust-lang.org/cargo/index.html)

```sh
cargo build
cargo run
cargo test
cargo install <crate>
```

> Cargo 中的软件包叫 `crates`

## 换源

编辑 `~/.cargo/config.toml`，添加以下内容：

```toml
[source.crates-io]
replace-with = 'ustc'

[source.ustc]
registry = "sparse+https://mirrors.ustc.edu.cn/crates.io-index/"
```

参考：[Rust Crates | USTC Mirror Help](https://mirrors.ustc.edu.cn/help/crates.io-index.html)