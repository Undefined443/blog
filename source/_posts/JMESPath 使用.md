最近在学习使用 AWS CLI，经常要用到 query 功能。AWS CLI 使用的查询语法是 JMESPath，因此特地在这里记录常用的 JMESPath 语法。

JMESPath 是一种查询语言，专门用于处理 JSON 对象。

JMESPath 规则和基本语法包括：

1. **字段访问**: 使用点 `.` 来访问 JSON 对象中的字段。例如，`MemoryInfo.SizeInMiB`。

2. **索引列表**: 通过索引来访问列表中的元素。索引是基于零的，例如 `Reservations[0].Instances[0].InstanceId`。

3. **管道表达式** (`|`): 用于将一个表达式的输出用作另一个表达式的输入。例如，`Reservations[].Instances[].Tags[] | [?Key=='Name'].Value`。

4. **切片操作**: 类似 Python 列表的切片，如 `Reservations[0:3]`，表示访问前三个元素。

5. **过滤**: 使用 `[]` 创建过滤表达式，找到符合特定条件的元素。例如，`Instances[?State.Name=='running']`。

6. **投影**: 即所谓的“映射操作”，对集合中的每个元素应用一个表达式。例如，`Instances[].{id: InstanceId, type: InstanceType}`。

7. **多选哈希**: 使用 `{}` 从多个字段中选择并重命名输出。例如，`{InstanceId: InstanceId, State: State.Name}`。

   例 1. 查询实例类型 `mac2-m2pro.metal` 的特定信息：

   ```sh
   aws ec2 describe-instance-types \
      --instance-type 'mac2-m2pro.metal' \
      --query 'InstanceTypes[*].{MemorySizeInMiB:MemoryInfo.SizeInMiB, NetworkPerformance:NetworkInfo.NetworkPerformance, ProcessorClockSpeed:ProcessorInfo.SustainedClockSpeedInGhz, SupportedArchitectures:SupportedArchitectures, vCPUs:VCpuInfo.DefaultVCpus}'
   ```

   解释：

      - `InstanceTypes[*]`: 访问数组 `InstanceTypes` 的所有元素
      - `{MemorySizeInMiB:MemoryInfo.SizeInMiB, ...}`: 对 `InstanceTypes` 中的每个元素，建立一个 JSON 对象，对象中每个 Key 的值自定义，Value 的值取自每个元素。

8. **函数**: JMESPath 还包含一组函数，例如 `contains()`, `starts_with()`, `sort()` 等，用于执行更复杂的操作。

See also: [jmespath.org](https://jmespath.org/)

中文版：[www.osgeo.cn](https://www.osgeo.cn/jmespath/)