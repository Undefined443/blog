## 安装

### Ubuntu

```sh
# 更新软件包索引
sudo apt update
# 安装辅助工具
sudo apt install apt-transport-https ca-certificates gnupg curl
# 导入 Google Cloud 公钥
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg --dearmor -o /usr/share/keyrings/cloud.google.gpg
# 添加 gcloud CLI 发行版 URI 作为软件包源
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
# 更新软件包索引
sudo apt update
# 安装 gcloud CLI
sudo apt install google-cloud-cli
```

初始化：

```sh
gcloud init
```

你将被提示使用浏览器打开登录链接并选择一个 Google Cloud 项目。

### macOS

前往官网 [安装最新的 gcloud CLI 版本](https://cloud.google.com/sdk/docs/install-sdk?hl=zh-cn#installing_the_latest_version)，下载适合你系统的安装包。

解压安装包并进入解压目录，然后运行：

```sh
# 安装
./google-cloud-sdk/install.sh
# 初始化
./google-cloud-sdk/bin/gcloud init
```

打开一个新的终端窗口以便配置生效。

参考：[安装 Google Cloud CLI](https://cloud.google.com/sdk/docs/install-sdk?hl=zh-cn#deb)