在 Java 中，您可以使用 `Scanner` 类从命令行读取输入。这个类属于 `java.util` 包，因此在使用之前您需要导入该包。

下面是一个如何从命令行读取输入的 Java 程序示例：

```java
import java.util.Scanner;  // 导入 Scanner 类

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);  // 创建 Scanner 对象

        System.out.print("请输入一些文本：");
        String input = scanner.nextLine();  // 读取用户输入的一行文本
        System.out.println("你输入的是：" + input);

        System.out.print("请输入一个整数：");
        int integerInput = scanner.nextInt();  // 读取用户输入的整数
        System.out.println("你输入的整数是：" + integerInput);

        System.out.print("请输入一个浮点数：");
        double doubleInput = scanner.nextDouble();  // 读取用户输入的浮点数
        System.out.println("你输入的浮点数是：" + doubleInput);

        scanner.close();  // 关闭 Scanner 对象
    }
}
```

在这个例子中：

1. 我们导入了 `java.util.Scanner`。
2. 在 `main` 方法中，我们创建了一个 `Scanner` 对象来读取 `System.in`（标准输入，即键盘）。
3. 使用 `nextLine()` 方法读取了一行文本。
4. 使用 `nextInt()` 方法读取了一个整数。
5. 使用 `nextDouble()` 方法读取了一个浮点数。
6. 最后，使用 `scanner.close()` 方法关闭了 `Scanner` 对象。

注意：
- 对于不同类型的输入，`Scanner` 类提供了不同的方法，如 `next()`, `nextInt()`, `nextDouble()` 等。
- 当使用 `next()` 方法读取单词时，它只会读取到空格之前的内容。如果您想要读取一整行，应该使用 `nextLine()` 方法。
- 使用完 `Scanner` 对象后，应该调用 `close()` 方法来释放与该对象关联的资源。
- 谨慎处理输入不匹配的异常。例如，如果试图使用 `nextInt()` 读取非整数类型的输入时，将会抛出 `InputMismatchException` 异常。在实际的应用程序中，您需要捕获并处理此类异常。
