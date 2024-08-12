在 Gradle 中构建一个包含所有依赖的 jar 包（通常被称为“fat jar”或者“uber jar”），你可以使用 shadowJar 插件来包含编译的类和依赖。

这里是一个基本的例子，使用 shadowJar 插件：

1. 首先，在你的 `build.gradle` 文件中引入 shadowJar 插件：
```gradle
plugins {
    id 'java'
    id 'com.github.johnrengelman.shadow' version '8.1.1'
}
```

2. 然后，可选地配置 shadowJar 任务，比如指定输出的 jar 文件的名字：
```gradle
shadowJar {
    archiveBaseName.set('myapp')  // jar 文件的基本名字
    archiveVersion.set('1.0.0')  // jar 文件的版本号
	archiveClassifier.set('')  // jar 文件的额外标签，这里设为空
}
```

3. 运行 shadowJar 任务：

```sh
gradle shadowJar
```

构建完成后，你会在 `build/libs/` 目录下找到名为 `myapp-1.0.0.jar` 的 fat jar 文件。这个 jar 包含了你的项目的所有依赖。