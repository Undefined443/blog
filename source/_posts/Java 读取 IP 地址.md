## 使用 InetAddress 类

可以利用 Java 自带的 `InetAddress` 类来检查一个字符串是否为有效的 IP 地址：

```java
import java.net.InetAddress;  // 导入 InetAddress 类
import java.net.UnknownHostException;  // 导入错误类

public class IPAddressUtil {
    // 测试输入的字符串参数 ip 是否为有效的 IP 地址
    public static boolean isValidIPAddress(String ip) {
        try {
            InetAddress address = InetAddress.getByName(ip);
            // 判断得到的 InetAddress 对象是否为字符串提供的原始 IP 地址
            return ip.equals(address.getHostAddress());
        } catch (UnknownHostException ex) {  // 识别出错，字符串 ip 不是有效的 IP 地址
            return false;
        }
    }

    // 测试方法
    public static void main(String[] args) {
        System.out.println(isValidIPAddress("192.168.1.1"));  // true
        System.out.println(isValidIPAddress("256.1.1.1"));  // false
    }
}
```

上面的方法既会检查 IPv4 地址，也会检查 IPv6 地址。`getByName()` 方法会解析提供的字符串并返回一个 `InetAddress` 对象，如果解析不成功（也就是说，地址无效或未知），则会抛出 `UnknownHostException` 异常。