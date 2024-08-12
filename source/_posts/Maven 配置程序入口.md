## 配置单个程序入口

[Exec Maven Plugin](https://mvnrepository.com/artifact/org.codehaus.mojo/exec-maven-plugin) 插件允许你在 Maven 生命周期中的某个阶段直接运行 Java 类。

在你的 `pom.xml` 文件中添加如下配置：

```xml
<project>
	...
		<build>
			...
			<plugins>
				...
				<plugin>
					<groupId>org.codehaus.mojo</groupId>
					<artifactId>exec-maven-plugin</artifactId>
					<!-- 请使用最新或适合你项目的版本 -->
					<version>3.2.0</version>
					<executions>
						<execution>
							<goals>
								<goal>java</goal>
							</goals>
						</execution>
					</executions>
					<configuration>
						<!-- 在这里配置你的主类 -->
						<mainClass>com.example.Main</mainClass>
						<!-- 在这里填写需要传递给 main 方法的命令行参数 -->
						<arguments>
							<argument>arg1</argument>
							<argument>arg2</argument>
							...
						</arguments>
					</configuration>
				</plugin>
				...
			</plugins>
			...
		</build>
		...
</project>
```

在这个例子中，`<mainClass>` 标签包含了你的主类的全路径，比如 `com.example.Main`。这假设 `com.example.Main` 中包含一个标准的 `main` 方法作为应用的入口。

一旦配置好，你可以使用以下命令运行你的应用：

```sh
mvn exec:java
```

Maven 会启动定义的主类。如果需要在 IDE 中运行程序，通常直接从 IDE 启动主类会比较方便。

如果你的项目是一个 web 应用程序，你可能会使用 `tomcat7-maven-plugin`、`jetty-maven-plugin` 或者其他适合的 Servlet 容器插件来运行你的 web 应用。

## 配置多个程序入口

配置多个程序入口的方法类似配置单个程序入口，但是你需要为每个程序入口设置单独的 `<execution>` 执行块，每个执行块都有其对应的 `<id>` 和 `<configuration>`：

```xml
<project>
	...
	<build>
		...
		<plugins>
			...
			<plugin>
				<groupId>org.codehaus.mojo</groupId>
				<artifactId>exec-maven-plugin</artifactId>
				<version>3.2.0</version>
				<executions>
					<!-- 配置运行 com.example.A 类 -->
					<execution>
						<!-- 在这里指定一个唯一的 ID -->
						<id>run-A</id>
						<configuration>
							<mainClass>com.example.A</mainClass>
							<!-- 入口类 A 的参数 -->
							<arguments>
								<argument>arg1</argument>
								<argument>arg2</argument>
								...
							</arguments>
						</configuration>
						<goals>
							<goal>java</goal>
						</goals>
					</execution>

					<!-- 配置运行 com.example.B 类 -->
					<execution>
					<!-- 入口类 B 的 ID -->
						<id>run-B</id>
						<configuration>
							<mainClass>com.example.B</mainClass>
							<!-- 入口类 B 的参数 -->
							<arguments>
								<argument>arg3</argument>
								...
							</arguments>
						</configuration>
						<goals>
							<goal>java</goal>
						</goals>
					</execution>
					...
				</executions>
			</plugin>
			...
		</plugins>
		...
	</build>
	...
</project>
```

在这个示例中，在 `<executions>` 中有了两个不同的 `<execution>` 块，每个执行块都有一个 `<id>` 和 `<configuration>`。通过这个配置，你可以执行两个不同的主类：

- 要运行 `com.example.A` 类，请执行：

```sh
mvn exec:java@run-A
```

- 要运行 `com.example.B` 类，请执行：

```sh
mvn exec:java@run-B
```

这里，`@run-A` 和 `@run-B` 是定义在 `<execution>` 块的 `<id>` 标签中的标识符。这些标识符用于通过 Maven 命令行区分和执行对应的主类。如果你没有指定 `<id>`，Maven 默认使用 `default-cli`，所以当你有多个执行块时指定 `<id>` 是非常重要的。