## 添加外部依赖

向你的 Maven / Gradle 项目添加依赖的过程可分为如下几步：

1. 搜索依赖

   1. 搜索你要安装的依赖，比如你需要 `MySQL Connector/J`，可以在谷歌搜索“MySQL Connector/J maven”（在你需要的依赖名后面加上“maven”），这样谷歌会为你推荐 Maven Repository（[mvnrepository.com](https://mvnrepository.com/)）的结果，我们大部分依赖都是从 Maven Repository 安装的。

      ![image](https://s2.loli.net/2024/07/07/m6RMUfvkJjY2OL4.png)

   2. 进入 Maven 仓库之后，你可能会看到若干版本选项，选择你想要的版本，点击版本号进入。

      ![image](https://s2.loli.net/2024/07/07/aXA3UmsN4ht8Fvf.png)

   3. 进入版本号详情页后，你会发现页面下面针对不同项目管理工具给出了不同的配置片段。如果你使用 Maven 管理项目，就选择 `Maven` 标签，如果你使用 Gradle 管理项目，就选择 `Gradle (xxx)` 标签。之后点击下面的配置片段，其中的代码会自动复制到你的剪贴板。

      ![image](https://s2.loli.net/2024/07/07/lA6CWziRtqsKUbS.png)

2. 将依赖配置片段放入项目配置文件

   1. 如果你使用 Maven 管理项目，那么你的项目配置文件是 `pom.xml`，将你复制到的 `<dependency>...</dependency>` 片段粘贴到 `pom.xml` 文件中的 `<dependencies>...</dependencies>` 标签之间。

      ```xml
      <project>
      	...
      	<dependencies>
      		<!-- 将你复制的配置片段粘贴到这里 -->
      	</dependencies>
      	...
      </project>
      ```

   2. 如果你使用 Gradle 管理项目，那么你的项目配置文件是 `app/build.gradle`，将你复制到的配置片段粘贴到 `app/build.gradle` 文件中的 `dependencies {...}` 代码块之间。

      ```gradle
      dependencies {
          // 将你复制的配置片段粘贴到这里
      }
      ```

3. 这样，我们的依赖就导入完成了。当我们编译项目的时候我们的项目管理工具会自动帮我们下载依赖并安装到合适位置。

关于 Gradle 依赖管理的详细说明可以查看这篇文章：[Java Gradle入门指南之依赖管理（添加依赖、仓库、版本冲突）](https://www.cnblogs.com/gzdaijie/p/5296624.html)

## 添加本地依赖

1. 创建 `libs` 目录：

   在你的 Maven 项目根目录下创建一个 `libs` 目录，并将你的 JAR 包放入其中。例如：

   ```
   my-maven-project/
   ├── libs/
   │   └── my-library.jar
   ├── src/
   ├── pom.xml
   ```

2. 配置 `pom.xml`：

   在 `pom.xml` 中配置 `systemPath` 来引用本地 JAR 包：

   ```xml
   <dependency>
   	<groupId>com.example</groupId>
   	<artifactId>my-library</artifactId>
   	<version>1.0.0</version>
   	<scope>system</scope>
   	<systemPath>${project.basedir}/libs/my-library.jar</systemPath>
   </dependency>
   ```

   请注意，在 `pom.xml` 中使用 `systemPath` 并不是推荐的做法，因为它会使项目的依赖管理变得不那么灵活。但是在某些情况下，这是一个快速解决问题的方法。
