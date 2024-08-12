## 项目和依赖分别打入独立 JAR 包

使用 [Maven Jar Plugin](https://mvnrepository.com/artifact/org.apache.maven.plugins/maven-jar-plugin) 插件，可以将项目自身单独打成一个 JAR 包，项目依赖的 JAR 包统一放置到指定目录。

在项目的 `pom.xml` 中添加如下配置：

```xml
<project>
	...
	<build>
		...
		<plugins>
			...
			<!-- Maven Jar Plugin -->
			<!-- 用于构建 jar 包 -->
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-jar-plugin</artifactId>
				<version>3.3.0</version>
				<configuration>
					<archive>
						<manifest>
							<addClasspath>true</addClasspath>
							<classpathPrefix>
								<!-- 如果 addClasspath 为 true，则下面填写 classpath 的目录前缀 -->
								lib/
							</classpathPrefix>
							<mainClass>
								<!-- 这里填写主类名 -->
								com.example.App
							</mainClass>
						</manifest>
					</archive>
				</configuration>
			</plugin>

			<!-- Maven Dependency Plugin -->
			<!-- 用于将依赖复制到指定位置 -->
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-dependency-plugin</artifactId>
				<version>3.6.1</version>
				<executions>
					<execution>
						<id>copy-dependencies</id>
						<phase>prepare-package</phase>
						<goals>
							<goal>copy-dependencies</goal>
						</goals>
						<configuration>
							<outputDirectory>
								<!-- 这里填写依赖输出目录，必须和上面填写的 classpathPrefix 相对应 -->
								${project.build.directory}/lib
							</outputDirectory>
						</configuration>
					</execution>
				</executions>
			</plugin>
			...
		</plugins>
		...
	</build>
	...
</project>
```

运行如下命令进行打包：

```sh
mvn package
```

输出结构（有省略）：

```
target
├── lib
│   ├── dependency-1.jar
│   ├── dependency-2.jar
│   └── dependency-3.jar
└── demo-1.0.jar
```

其中 `demo-1.0.jar` 是我们项目的 JAR，`lib` 目录下是项目依赖的 JAR。

使用方法：

```sh
java -jar demo-1.0.jar
```

使用时，`lib` 目录和构建成果 `demo-1.0.jar` 必须处于同一目录下。

## 项目和依赖打入同一个 JAR 包

使用 [Maven Shade Plugin](https://mvnrepository.com/artifact/org.apache.maven.plugins/maven-shade-plugin)，可以将项目及其依赖打包到单个 JAR 文件中。

在项目的 `pom.xml` 中添加如下配置：

```xml
<project>
	...
	<build>
		...
		<plugins>
			...
			<!-- Maven Shade Plugin -->
			<!-- 将项目输出合并到一个包含依赖项、模块、站点文档和其他文件的单个可分发存档中 -->
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-shade-plugin</artifactId>
				<version>3.6.0</version>
				<executions>
					<execution>
						<phase>package</phase>
						<goals>
							<goal>shade</goal>
						</goals>
						<configuration>
							<transformers>
								<transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
									<mainClass>com.example.App</mainClass> <!-- 替换为你的主类 -->
								</transformer>
							</transformers>
						</configuration>
					</execution>
				</executions>
			</plugin>
			...
		</plugins>
		...
	</build>
	...
</project>
```

运行如下命令进行打包：

```sh
mvn package
```

输出结构（有省略）：

```
target
├── demo-1.0-jar-with-dependencies.jar
└── demo-1.0.jar
```

其中 `demo-1.0.jar` 是不带依赖的 JAR 包，`demo-1.0-jar-with-dependencies.jar` 是带依赖的 JAR 包。

使用方法：

```sh
java -jar demo-1.0-jar-with-dependencies.jar
```

> 参考：[Maven - 打包可执行 jar 包 | 简书](https://www.jianshu.com/p/0d85d0539b1a)