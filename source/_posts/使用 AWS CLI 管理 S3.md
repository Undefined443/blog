S3 是 AWS 的对象存储服务

S3: Simple Storage Service

### 创建桶

使用 `aws s3 mb` 命令创建新的 S3 桶。您需要提供一个全球唯一的桶名称和创建桶的区域。

```sh
aws s3 mb 's3://your-bucket-name' --region 'your-region'
```

桶名称应遵循以下规则：

- 必须全球唯一。
- 必须是小写。
- 不得格式化为 IP 地址（例如，192.168.5.4）。
- 必须在 3 到 63 字符之间。
- 不能包含下划线，以连字符开头或结尾，且不能有连续的连字符。

### 删除桶

```sh
aws s3 rb 's3://your-bucket-name' --force
```

### 常用操作

S3 桶的操作和我们平时使用 Bash 对文件进行操作的命令基本一致。这里只简单列出其中几条命令。

```sh
# 列出桶中所有文件
aws s3 ls --recursive 's3://your-bucket-name/'

# 文件拷贝
aws s3 cp 'local-path' 's3://your-bucket-name/remote-path'

# 文件删除
aws s3 rm 's3://your-bucket-name/file'

# 文件同步
aws s3 sync 'local-path' 's3://your-bucket-name/remote-path'
```

### 为文件生成预览链接

```sh
aws s3 presign 's3://your-bucket-name/path/to/file' --expires-in 3600
```

- `--expires-in` 设定过期时间

presign，预签名。实际上是我们使用自己的身份提前解锁了文件，并生成了解锁链接。

所有支持的操作：

| 命令名     | 功能             |
|:----------:|------------------|
| `cp`       | 拷贝             |
| `ls`       | 列表             |
| `mb`       | 创建桶           |
| `mv`       | 移动 / 重命名    |
| `presign`  | 预签名           |
| `rb`       | 删除桶           |
| `rm`       | 删除             |
| `sync`     | 同步             |
| `website`  | 设置桶的网站配置 |

See also:

- [aws s3 help](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3/index.html)
- [aws s3api help](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3api/index.html)
- [aws s3control help](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3control/index.html)

其中 `s3` 是 AWS S3 的高级 API，`s3api` 和 `s3control` 分别是 S3 和 S3 Glacier 的低级 API，它们提供了更多的配置选项和更详细的控制。