[MMDB-Lookup | GitHub](https://github.com/Undefined443/MMDB-Lookup)

`Lookup.java`:

```java
import java.io.File;
import java.net.InetAddress;

import com.fasterxml.jackson.databind.JsonNode;
import com.maxmind.db.Reader;

public class Lookup {
    public static void main(String[] args) throws Exception {
        File database = new File("resources/Country.mmdb");
        Reader reader = new Reader(database);
        // 查询
        InetAddress address = InetAddress.getByName("114.114.114.114");
        JsonNode response = reader.get(address);
        System.out.println(response);
        reader.close();
    }
}
```

需要引入 [`Maxmind DB`](https://github.com/maxmind/MaxMind-DB-Reader-java) 和 [`Jackson Databind`](https://github.com/FasterXML/jackson-databind) 包

`pom.xml`:

```xml
<project>
	...
	<dependencies>
		...
		<dependency>
			<groupId>com.maxmind.db</groupId>
			<artifactId>maxmind-db</artifactId>
			<version>1.2.2</version>
		</dependency>
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-databind</artifactId>
			<version>2.16.1</version>
		</dependency>
		...
	</dependencies>
	...
</project>
```

运行结果样例：

```json
{"country":{"is_in_european_union":false,"names":{"de":"China","ru":"Китай","pt-BR":"China","ja":"中国","en":"China","fr":"Chine","zh-CN":"中国","es":"China"},"iso_code":"CN","geoname_id":1814991}}
```

参考：

- [MMDB ip 地址库操作 | CSDN](https://blog.csdn.net/jediael_lu/article/details/76686968)
- [GeoIP2 and GeoLite2 Database Documentation | MaxMind Developers](https://dev.maxmind.com/geoip/docs/databases#api-clients)