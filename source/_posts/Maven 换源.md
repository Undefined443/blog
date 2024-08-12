`~/.m2/settings.xml`

```xml
<settings>
  ...
  <mirrors>
    ...
      <mirror>
      <id>alimaven</id>
      <mirrorOf>central</mirrorOf>
      <name>aliyun maven</name>
      <url>https://maven.aliyun.com/repository/central</url>
    </mirror>
    <mirror>
      <id>ali.snapshots.https</id>
      <mirrorOf>apache.snapshots.https</mirrorOf>
      <name>aliyun snapshots</name>
      <url>https://maven.aliyun.com/repository/apache-snapshots</url>
    </mirror>
    ...
  </mirrors>
  ...
</settings>
```

Maven 配置模板下载方法：

进入 [Download Apache Maven](https://maven.apache.org/download.cgi) 页面，下载一个 Maven 二进制压缩包（如 `apache-maven-x.x.x-bin.tar.gz` 或 `apache-maven-x.x.x-bin.zip`），解压并打开，配置模板位于 `conf/settings.xml`