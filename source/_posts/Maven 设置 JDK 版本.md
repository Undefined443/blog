Maven 设置 JDK 版本是通过 [Apache Maven Compiler Plugin](https://mvnrepository.com/artifact/org.apache.maven.plugins/maven-compiler-plugin) 插件实现的。它用于编译项目的源代码。

## 方法一

有时候你可能需要将某个项目编译到与当前使用的 JDK 版本不同的语言版本。你可以在编译时为 `javac` 工具添加 `-source` 和 `-target` 选项来实现这样的功能。也可以配置 Apache Maven Complier Plugin 插件以在编译过程中提供这两个选项。

例如，如果你想要使用 Java 8 的语言特性（`-source 1.8`）并且还希望编译后的类与 JVM 1.8 兼容（`-target 1.8`），你可以在 `pom.xml` 文件中添加下面两个属性，这是插件参数的默认属性名称：

```xml
<project>
	...
	<properties>
		<maven.compiler.source>1.8</maven.compiler.source>
		<maven.compiler.target>1.8</maven.compiler.target>
	</properties>
	...
</project>
```

或者直接配置插件（效果同上）：

```xml
<project>
	...
	<build>
		...
		<plugins>
			...
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.8.1</version>
				<configuration>
					<source>1.8</source>
					<target>1.8</target>
				</configuration>
			</plugin>
			...
		</plugins>
		...
	</build>
	...
</project>
```

如果你没有配置 `source` 和 `target` 属性，默认值是 `1.8`

`source` 选项指定了你的源代码中能够使用什么版本的语言特性，`target` 选项指定了编译出来的 class 文件能够在什么版本的 JVM 上运行。一般来说这两个值设置成一样即可。

[Setting the -source and -target of the Java Compiler | Apache Maven](https://maven.apache.org/plugins/maven-compiler-plugin/examples/set-compiler-source-and-target.html)

## 方法二（推荐）

从 JDK 9 开始， `javac` 工具可以接受 `--release` 选项，用于指定要针对哪个 Java SE 版本构建项目。例如，你安装了 JDK 11 并且被 Maven 使用，但你想要针对 Java 8 构建项目。`--release` 选项确保代码编译遵循指定版本的语言规则，并且生成的类也针对该版本以及该版本的公共 API。这意味着，与 `-source` 和 `-target` 选项不同，编译器将在你使用在先前 Java SE 版本中不存在的 API 时检测并生成错误。

自 Apache Maven Complier Plugin 插件的 3.6 版本起，可以通过设置 `<maven.compiler.release>` 属性提供此选项：

```xml
<project>
	...
	<properties>
		<maven.compiler.release>8</maven.compiler.release>
	</properties>
	...
</project>
```

或者直接配置插件（效果同上）：

```xml
<project>
	...
	<build>
		...
		<plugins>
			...
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.12.1</version>
				<configuration>
					<release>8</release>
				</configuration>
			</plugin>
			...
		</plugins>
		...
	</build>
	...
</project>
```

[Setting the --release of the Java Compiler | Apache Maven](https://maven.apache.org/plugins/maven-compiler-plugin/examples/set-compiler-release.html)