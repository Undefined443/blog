Maven 是一个基于 Project Object Model（POM）的项目管理和构建工具，主要用于 Java 项目。

## 创建 Java 项目

```sh
# 使用 archetype 插件构建 Java 项目
mvn archetype:generate
# 你也可以在命令中直接提供项目参数
mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

参考：[Maven 使用 Archetype（原型）插件构建 Java 项目 | C 语言中文网](https://c.biancheng.net/maven2/archetype.html)

## 依赖管理

关于依赖管理你可以直接查看我的这篇博客：[Maven / Gradle 依赖管理](https://www.cnblogs.com/Undefined443/p/18050499)

## 生命周期、阶段和目标

Maven 通过 `lifecycle`（生命周期）、`phase`（阶段）和 `goal`（目标）来提供标准的构建流程。

最常用的构建命令是指定 `phase`，然后让 Maven 执行到指定的 `phase`：

```sh
mvn clean          # 清理 Maven 的输出目录
mvn clean compile  # 编译代码到输出目录
mvn clean test     # 执行测试用例
mvn clean package  # 创建项目 jar/war 包，最常用的命令
```

通常情况，我们总是执行 `phase` 默认绑定的 `goal`，因此不必指定 `goal`。

实际上，执行每个 `phase`，都是通过某个插件（plugin）来执行的。比如说，Maven 本身其实并不知道如何执行 `compile`，它只是负责找到对应的 `compiler` 插件，然后执行默认的 `compiler:compile` 这个 `goal` 来完成编译。所以，使用 Maven，实际上就是配置好需要使用的插件，然后通过 `phase` 调用它们。

参考：[Maven 插件绑定 | C 语言中文网](https://c.biancheng.net/maven2/plugin.html)

See also:

- [Runoob 教程](https://www.runoob.com/maven/maven-creating-project.html)
- [廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/1252599548343744/1309301196980257)
- [C语言中文网](https://c.biancheng.net/maven2/project.html)