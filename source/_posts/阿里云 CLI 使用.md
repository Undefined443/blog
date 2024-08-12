## 安装

macOS:

```sh
brew install aliyun-cli
```

Linux:

```sh
wget https://aliyuncli.alicdn.com/aliyun-cli-linux-latest-amd64.tgz
tar xzvf aliyun-cli-linux-3.0.16-amd64.tgz
sudo cp aliyun /usr/local/bin
```

## 配置

配置文件位置在 `~/.aliyun`

提前[创建阿里云 AccessKey](https://help.aliyun.com/zh/ram/user-guide/create-an-accesskey-pair)

配置：

```sh
aliyun configure
```

[地域和可用区 | 阿里云帮助中心](https://help.aliyun.com/document_detail/40654.html)

启用自动补全：

```sh
aliyun auto-completion
```

关闭自动补全：

```sh
aliyun auto-completion --uninstall
```

## 使用

### ECS

```sh
aliyun ecs CreateInstance
    --InstanceName myvm1
    --ImageId centos_7_03_64_40G_alibase_20170625.vhd
    --InstanceType ecs.n4.small
    --SecurityGroupId sg-xxxxxx123
    --VSwitchId vsw-xxxxxx456
    --InternetChargeType PayByTraffic
    --Password xxx
```

命令可以通过 [OpenAPI 门户](https://next.api.aliyun.com/)生成

参考：[阿里云 CLI | 阿里云帮助中心](https://help.aliyun.com/zh/cli/)