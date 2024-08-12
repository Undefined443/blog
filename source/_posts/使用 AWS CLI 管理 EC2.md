## EC2

EC2 是 AWS 的云服务器服务

EC2: Elastic Compute Cloud

### 创建实例

1. **选择一个系统镜像（AMI）**：

   - AMI（Amazon Machine Image）定义了启动实例所需的信息。您可以使用 AWS CLI 查询：

      ```sh
      # 查询 Ubuntu 24.04 x86_64
      aws ec2 describe-images \
          --owners 'amazon' \
          --query 'Images[*].[ImageId,Name]' \
          --filters "Name=name,Values=*ubuntu/*22.04*" "Name=architecture,Values=x86_64"

      # 查询 Windows Server 2022 Chinese Simplified
      aws ec2 describe-images \
          --owners 'amazon' \
          --query 'Images[*].[ImageId,Name]' \
          --filters 'Name=name,Values=*Windows_Server-2022-Chinese_Simplified*'

      # 查询 macOS 14
      aws ec2 describe-images \
          --owners 'amazon' \
          --query 'Images[*].[ImageId,Name]' \
          --filters 'Name=name,Values=*macos-14*'
      ```

      替换查询参数以匹配您想要的操作系统。

   - 查看 AMI 详细信息：

      ```sh
      aws ec2 describe-images \
          --image-id 'ami-01bef798938b7644d'
      ```

   > 不同地区的可用镜像不同

   [aws ec2 describe-images help](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ec2/describe-images.html)

2. **选择一个实例类型**：

   实例类型就是云服务器的硬件配置。例如 `t2.micro`，`c7i.xlarge`，`mac2.metal` 等。

   [Amazon EC2 实例类型](https://aws.amazon.com/cn/ec2/instance-types/?nc1=h_ls)

   查询实例类型列表：

   ```sh
   # 查找 C7 系列的所有 arm64 配置
   aws ec2 describe-instance-types \
       --query 'InstanceTypes[*].[InstanceType]'
       --filters 'Name=instance-type,Values=c7*' 'Name=processor-info.supported-architecture,Values=arm64'

   # 查找适用于 macOS 的所有配置
   aws ec2 describe-instance-types \
       --query 'InstanceTypes[*].[InstanceType]'
       --filters 'Name=instance-type,Values=mac*'
   ```

   查看实例类型详细信息：

   ```sh
   aws ec2 describe-instance-types \
       --instance-type 't2.micro' \
       --query 'InstanceTypes[*].{FreeTierEligible:FreeTierEligible,MemorySizeInMiB:MemoryInfo.SizeInMiB, NetworkPerformance:NetworkInfo.NetworkPerformance, ProcessorInfo:ProcessorInfo, vCPUs:VCpuInfo.DefaultVCpus}'
   ```

3. **创建一个密钥对（如果还没有的话）**：

   - 生成新的密钥对：

      ```sh
      aws ec2 create-key-pair \
          --key-name 'MyKeyPair' \
          --query 'KeyMaterial' \
          --output 'text' > MyKeyPair.pem
      ```

      切记保存生成的 `.pem` 文件，并设置正确的文件权限：

      ```sh
      chmod 400 MyKeyPair.pem
      ```

   - 或者上传已有的公钥：

      ```sh
      aws ec2 import-key-pair \
          --key-name 'MyKeyPair' \
          --public-key-material 'fileb://~/.ssh/id_rsa.pub'
      ```

   - 如果你在 Docker 容器中运行 AWS CLI，则应该运行如下命令：

      ```sh
      PUB_KEY=$(cat ~/.ssh/id_rsa.pub | base64 | tr -d '\n')
      # 上传公钥
      aws ec2 import-key-pair \
          --key-name "MyKeyPair" \
          --public-key-material "$PUB_KEY"
      ```

   - 删除密钥的方法：

      ```sh
      aws ec2 delete-key-pair --key-name 'MyKeyPair'
      ```

4. **启动实例**：

   ```sh
   aws ec2 run-instances \
       --image-id 'ami-04b70fa74e45c3917' \
       --count 1 \
       --instance-type 'c7i.xlarge' \
       --key-name 'MyKeyPair' \
       --security-groups 'my-security-group' \
       --block-device-mappings '[{"DeviceName":"/dev/sda1","Ebs":{"VolumeSize":30}}]' \
       --query 'Instances[*].[InstanceType,KeyName,Architecture]'
   ```

   这将会创建一个操作系统为 Ubuntu 24.04 并带有 30 GB 存储空间的实例。

   替换 `ami-04b70fa74e45c3917` 为您选择的 AMI ID，`MyKeyPair` 为您的密钥对名字，`my-security-group` 为您的安全组。

5. **查看实例**

   ```sh
   aws ec2 describe-instances \
       --query 'Reservations[*].Instances[*].[InstanceId,InstanceType,Architecture,State.Name,PublicIpAddress]'
   ```

   这会显示您所有实例的基本信息，如实例 ID、实例类型、架构、状态和公共 IP。

### 终止实例

1. 查找实例 ID

   如果您不知道要终止的实例的ID，可以首先列出您的所有实例以及它们的状态，以便找到正确的实例ID：

   ```sh
   aws ec2 describe-instances \
       --query 'Reservations[*].Instances[*].[InstanceId,InstanceType,Architecture,State.Name,PublicIpAddress]'
   ```

   上述命令显示了所有实例的 ID、类型、架构、状态和公共 IP。查找您需要终止的实例的 ID。

2. 终止实例

   一旦您有了实例 ID，可以使用以下命令终止实例：

   ```sh
   aws ec2 terminate-instances \
       --query 'TerminatingInstances[*].[InstanceId,CurrentState.Name]' \
       --instance-ids 'i-1234567890abcdef0'
   ```

   在上述命令中，`i-1234567890abcdef0` 是您要终止的实例的 ID。如果您有多个实例需要终止，可以一次列出所有实例 ID：

   ```sh
   aws ec2 terminate-instances \
       --query 'TerminatingInstances[*].[InstanceId,CurrentState.Name]' \
       --instance-ids 'i-1234567890abcdef0' 'i-abcdef1234567890'
   ```

3. 确认实例已终止

   终止命令后，您可以检查实例的状态来确认它已被终止：

   ```sh
   aws ec2 describe-instances \
       --query 'Reservations[*].Instances[*].State.Name' \
       --instance-ids 'i-1234567890abcdef0'
   ```

   这将返回实例的当前状态，如果实例已成功终止，则应返回 `"terminated"`。

### 更改实例类型

1. 停止 EC2 实例

   在更改实例类型之前，您需要先停止实例。

   ```sh
   aws ec2 stop-instances \
       --instance-ids 'i-1234567890abcdef0'
   ```

   确认实例完全停止。您可以使用以下命令检查实例状态：

   ```sh
   aws ec2 describe-instances \
       --instance-ids 'i-1234567890abcdef0' \
       --query 'Reservations[*].Instances[*].[State.Name]'
   ```

2. 更改实例类型

   停止实例后，使用以下命令更改实例类型：

   ```sh
   aws ec2 modify-instance-attribute \
       --instance-id 'i-1234567890abcdef0' \
       --instance-type '{"Value": "m5.large"}'
   ```

3. 启动实例

   完成实例类型更改后，启动实例：

   ```sh
   aws ec2 start-instances --instance-ids 'i-1234567890abcdef0'
   ```

### 重启实例

```sh
aws ec2 reboot-instances --instance-ids 'i-1234567890abcdef0'
```

## EBS

EBS（Elastic Block Store）

### 创建 EBS 卷

1. 创建一个新的 EBS 卷：

   ```sh
   aws ec2 create-volume \
       --availability-zone us-east-1a \
       --size 30 \
       --volume-type "gp3"
   ```

   - `--availability-zone`: 卷被创建的可用区。这通常是 EC2 实例所在的同一可用区。
   - `--size`: 卷的大小，以 GB 为单位。
   - `--volume-type`: 卷的类型，比如 `gp2` (通用 SSD), `io1` (高性能 SSD), 等等。

   此命令将返回卷的详细信息，包括卷的 ID（例如`vol-123abc456def7890`）。此 ID 稍后用于将卷附加到实例。

2. 将 EBS 卷附加到 EC2 实例

   一旦你拥有了 EBS 卷的 ID 和目标 EC2 实例的 ID，你可以使用以下命令将这个卷附加到实例：

   ```sh
   aws ec2 attach-volume \
       --volume-id 'vol-123abc456def7890' \
       --instance-id 'i-01234abcd5678efgh' \
       --device '/dev/sdf'
   ```

   - `--volume-id`: 你在上一步中创建的卷 ID。
   - `--instance-id`: 你希望附加新卷的 EC2 实例的 ID。
   - `--device`: 该设备在实例中的名称。确保这个设备名没有被实例中已有的设备使用。

3. 格式化和挂载卷（在 EC2 实例上）

   附加卷后，你需要登录到你的 EC2 实例并进行格式化及挂载：

   ```sh
   sudo mkfs -t ext4 /dev/xvdf  # 格式化卷
   sudo mkdir /mnt/myvolume     # 创建挂载点
   sudo mount /dev/xvdf /mnt/myvolume # 挂载卷
   ```

   > 替换 `/dev/xvdf` 和挂载点目录 `/mnt/myvolume` 为你实际需要的配置。

### 删除 EBS 卷

1. 分离 EBS 卷

   如果卷仍附着在某个 EC2 实例上，你需要首先将其分离。使用此命令分离卷：

   ```sh
   aws ec2 detach-volume --volume-id 'vol-xxxxxxx'
   ```

   - `--volume-id`: 要分离的卷的 ID。

   分离命令可能需要几秒钟到几分钟的时间来处理，具体取决于卷的当前状态和它是否在活跃使用中。

2. 检查卷的状态

   在尝试删除之前，确认卷的状态是 `available`。这意味着卷已经成功分离并准备好被删除。使用以下命令来检查卷的状态：

   ```sh
   aws ec2 describe-volumes --volume-ids 'vol-xxxxxxx'
   ```

   查看输出结果中的 `State` 字段，确认其值为 `available`。如果状态显示为 `in-use`，则说明卷仍然在被实例使用，应重新检查是否已正确分离。

3.  删除 EBS 卷

   一旦卷的状态为 `available`，你就可以安全地删除它。使用以下命令删除卷：

   ```sh
   aws ec2 delete-volume --volume-id 'vol-xxxxxxx'
   ```

   - `--volume-id`: 此前分离的卷的 ID。

   执行此命令后，卷将被删除，并且与之相关联的存储资源将被释放。

## 安全组

### 添加入站规则

1. 查询安全组 ID

   ```sh
   aws ec2 describe-security-groups --query 'SecurityGroups[*].[GroupId,GroupName]'
   ```

2. 添加入站规则

   ```sh
   aws ec2 authorize-security-group-ingress \
       --group-id 'GROUP_ID' \
       --protocol tcp \
       --port '3389' \
       --cidr '0.0.0.0/0'
   ```

3. 检查安全组规则

   ```sh
   aws ec2 describe-security-groups --group-ids 'GROUP_ID'
   ```

### 删除入站规则

```sh
aws ec2 revoke-security-group-ingress \
    --group-id 'GROUP_ID' \
    --protocol tcp \
    --port '3389' \
    --cidr '0.0.0.0/0'
```

See also:

- [JMESPath 入门](https://www.cnblogs.com/Undefined443/p/18170293)
- [Launch, list, and terminate Amazon EC2 instances](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-ec2-instances.html#launching-instances)
- [aws ec2 help](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ec2/index.html#cli-aws-ec2)