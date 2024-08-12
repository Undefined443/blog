## 安装

### macOS

使用 Homebrew：

```sh
brew install awscli
```

手动安装：

```sh
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
```

验证安装：

```sh
aws --version
```

卸载：

```sh
# 删除符号链接
sudo rm /usr/local/bin/aws
sudo rm /usr/local/bin/aws_completer
# 删除主安装文件夹
sudo rm -rf /usr/local/aws-cli
# 删除配置信息（可选）
rm -rf ~/.aws/
```

### Linux

```sh
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

更新：

```sh
sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli --update
```

验证安装：

```sh
aws --version
```

Linux 版的卸载的方式与 macOS 版相同。

参考：

1. [Install or update to the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
2. [Uninstall the AWS CLI version 2](https://docs.aws.amazon.com/cli/latest/userguide/uninstall.html)

### Docker

```sh
# 从 Amazon ECR Public 安装
docker pull public.ecr.aws/aws-cli/aws-cli
# 从 Docker Hub 安装
docker pull amazon/aws-cli
```

缩短 `docker run` 命令：

```sh
# Amazon ECR Public
alias aws='docker run --rm -it -v ~/.aws:/root/.aws -v $(pwd):/aws public.ecr.aws/aws-cli/aws-cli'
# Docker Hub
alias aws='docker run --rm -it -v ~/.aws:/root/.aws -v $(pwd):/aws amazon/aws-cli'
```

### 使用 CloudShell

AWS 在网页控制台中提供了免费的基于 Amazon Linux 的 CloudShell 以供使用。由于 CloudShell 的主机托管在 AWS，所以查询信息的速度可能要比本地运行的 AWS CLI 要快一点。

## 获取帮助

```sh
aws help
aws <command> help
```

[AWS CLI Command Reference](https://awscli.amazonaws.com/v2/documentation/api/latest/index.html#)

参考：[Run the AWS CLI from the official Amazon ECR Public or Docker images](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-docker.html)

## 配置

- 配置访问密钥、默认区域以及默认输出格式：

   ```sh
   $ aws configure
   AWS Access Key ID [None]:
   AWS Secret Access Key [None]:
   Default region name [None]:  # us-east-1 | ap-northeast-1 | ap-east-1
   Default output format [None]: # table | json
   ```

   获取访问密钥：[IAM 控制面板](https://console.aws.amazon.com/iam/home)

   如果在执行 `aws` 命令时不想使用默认区域，可以在命令后加上 `--region xx-xxxx-x` 选项

- 也可以每项单独设置：

   ```sh
   aws configure set aws_access_key_id 'xxx'
   aws configure set aws_secret_access_key 'xxx'
   aws configure set default.region 'us-east-1'
   ```

- 或者手动编辑凭据和配置文件

   AWS CLI 配置文件位于 `~/.aws` 目录。

   有关地区和输出格式的设置位于 `~/.aws/config`：

   ```ini
   [default]
   region = us-east-1
   output = table
   ```

   有关帐户 Access Key ID 和 Secret Access Key 的信息位于 `~/.aws/credentials`：

   ```ini
   [default]
   aws_access_key_id = xxx
   aws_secret_access_key = xxx
   ```

参考：[Set up the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-quickstart.html)

See also: [aws configure help](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/configure/index.html)