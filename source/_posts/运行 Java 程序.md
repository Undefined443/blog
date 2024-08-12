Java 程序实际上就是我们编译好的 Java 类文件。运行 Java 程序就是运行 Java 类的 `main` 函数。

## 编译并运行 Java 文件

源文件：

```java
package com.example;

public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

项目目录结构：

```
my-project
└── com
    └── example
        └── HelloWorld.java
```

编译和运行：

```sh
# 在项目根目录运行下面的命令
javac com/example/HelloWorld.java  # 编译。获得 Java 字节码文件 HelloWorld.class
java com.example.HelloWorld        # 运行 HelloWorld 类的 main 函数
```

## 运行 JAR 文件

大多数时候双击 JAR 文件就可以直接运行 JAR 文件。

也可以使用以下命令运行 JAR 文件：

```sh
java -jar myapp.jar
```

实际上，JAR 文件就是一个 ZIP 压缩包，里面存放着编译好的 `.class` 文件。因此，也可以自己指定 JAR 文件中的 `.class` 文件执行：

```sh
java -cp myapp.jar com.example.HelloWorld
```

## 使用 classpath

如果你的程序使用了外部库，那么在编译和运行时，你还需要包含这些库的路径到 `classpath` 中。如：`java -cp path/to/classfile HelloWorld`。路径间用 `:` 分隔（如果不加 `-cp` 选项，默认的 `classpath` 为当前目录 `.`）

> `classpath` 告诉了 Java 解释器在哪里查找类

例：

我写了一个 `maxmind.IpDemo`（下称 `IpDemo`）类，该类的 `.class` 文件位于项目根目录的 `maxmind` 目录下，该类中使用了 `com.fasterxml.jackson.databind.JsonNode`（下称 `JsonNode`）类以及 `com.maxmind.db.Reader` （下称 `Reader`）类。

源代码如下：

```java
package maxmind;

import java.io.File;
import java.net.InetAddress;

import com.fasterxml.jackson.databind.JsonNode;
import com.maxmind.db.Reader;

public class IpDemo {
    public static void main(String[] args) throws Exception {
        File database = new File("resources/Country.mmdb");
        Reader reader = new Reader(database);
        InetAddress address = InetAddress.getByName("114.114.114.114");
        JsonNode response = reader.get(address);
        System.out.println(response);
        reader.close();
    }
}
```

其中 `JsonNode` 类及其依赖类分别位于 `jackson-databind-2.16.1.jar`、`jackson-core-2.16.1.jar`、`jackson-annotations-2.16.1.jar` 三个 JAR 包中；

`Reader` 类位于 `maxmind-db-1.2.1.jar` 一个 JAR 包中；

这些 JAR 包位于 Maven 的包仓库的某处。

因此，在运行 `maxmind.IpDemo` 类时你需要添加如下 `classpath`：

- `JsonNode` 类及其依赖类的三个 `classpath`，即三个 JAR 包的位置
- `Reader` 类的一个 `classpath`，即一个 jar 包的位置
- `IpDemo` 类自身的 `classpath`，即项目根目录

使用如下命令运行 `maxmind.IpDemo` 类：

```sh
java -cp \
    $M2_HOME/com/fasterxml/jackson/core/jackson-databind/2.16.1/jackson-databind-2.16.1.jar: \
    $M2_HOME/com/fasterxml/jackson/core/jackson-core/2.16.1/jackson-core-2.16.1.jar: \
    $M2_HOME/com/fasterxml/jackson/core/jackson-annotations/2.16.1/jackson-annotations-2.16.1.jar: \
    $M2_HOME/com/maxmind/db/maxmind-db/1.2.1/maxmind-db-1.2.1.jar: \
    . \
        maxmind.IpDemo
```

如果你少写了其中一个 `classpath`（比如说少写了 `jackson-annotations-2.16.1.jar` 包的位置），则可能出现如下错误：

```
Exception in thread "main" java.lang.NoClassDefFoundError: com/fasterxml/jackson/annotation/JsonView
	at com.fasterxml.jackson.databind.introspect.JacksonAnnotationIntrospector.<clinit>(JacksonAnnotationIntrospector.java:37)
	at com.fasterxml.jackson.databind.ObjectMapper.<clinit>(ObjectMapper.java:400)
	at com.maxmind.db.Decoder.<clinit>(Decoder.java:28)
	at com.maxmind.db.Reader.<init>(Reader.java:131)
	at com.maxmind.db.Reader.<init>(Reader.java:116)
	at com.maxmind.db.Reader.<init>(Reader.java:66)
	at com.maxmind.db.Reader.<init>(Reader.java:53)
	at maxmind.IpDemo.main(IpDemo.java:12)
Caused by: java.lang.ClassNotFoundException: com.fasterxml.jackson.annotation.JsonView
	at java.base/jdk.internal.loader.BuiltinClassLoader.loadClass(BuiltinClassLoader.java:641)
	at java.base/jdk.internal.loader.ClassLoaders$AppClassLoader.loadClass(ClassLoaders.java:188)
	at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:525)
	... 8 more
```

表示找不到类定义（即找不到类的 `.class` 文件）

这里的错误表示找不到 `com.fasterxml.jackson.annotation.JsonView` 类的定义。根据类名我们猜测它应该位于 `jackson-annotations-2.16.1.jar` 包中，因此我们需要在 `classpath` 中添加 `jackson-annotations-2.16.1.jar` 包的位置。

如果出现错误：

```
错误: 找不到或无法加载主类 maxmind.IpDemo
原因: java.lang.ClassNotFoundException: maxmind.IpDemo
```

则表示你没有添加 `IpDemo` 类的路径，即项目根目录 `.`。