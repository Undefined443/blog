## dig

`dig`（Domain Information Groper）是一个用于 DNS 查询的命令行工具，广泛用于查看域名系统的相关信息。

### 基本用法

```sh
# 查询域名的 A 记录（IPv4 地址）：
dig example.com

# 查询指定 DNS 服务器的 A 记录：
dig @dns-server-ip example.com

# 指定查询记录类型（例如，查询 MX 记录）：
dig example.com MX

# 递归查询：
dig +recurse example.com

# 反向 DNS 查找，也称为 PTR 记录查询（查询 IP 对应的域名）：
dig -x 192.168.1.1
```

### 输出控制

```sh
# 只显示回答部分（不显示头部和其他信息）：
dig +short example.com

# 以人类可读的格式显示结果：
dig +noall +answer example.com
```

### 其他选项

```sh
# 指定端口号：
dig example.com -p 53

# 显示更详细的信息（调试模式）：
dig +trace example.com

# 显示详细的通信信息：
dig +stats example.com
```

### 使用示例

```sh
$ dig @114.114.114.114 registry-1.docker.io

; <<>> DiG 9.9.4-RedHat-9.9.4-38.el7_3.2 <<>> @114.114.114.114 registry-1.docker.io
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 17709
;; flags: qr rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;registry-1.docker.io.          IN      A

;; ANSWER SECTION:
registry-1.docker.io.   54      IN      A       54.242.59.189
registry-1.docker.io.   54      IN      A       3.215.51.67
registry-1.docker.io.   54      IN      A       54.83.42.45

;; Query time: 16 msec
;; SERVER: 114.114.114.114#53(114.114.114.114)
;; WHEN: Mon Aug 29 14:20:08 CST 2022
;; MSG SIZE  rcvd: 97
```

1. **命令头部信息：**

   ```
   ; <<>> DiG 9.9.4-RedHat-9.9.4-38.el7_3.2 <<>> @114.114.114.114 registry-1.docker.io
   ```

   - `DiG 9.9.4-RedHat-9.9.4-38.el7_3.2`：指明了使用的 `dig` 版本。
   - `@114.114.114.114`：指定了查询的 DNS 服务器地址为 114.114.114.114。
   - `registry-1.docker.io`：指定了查询的域名。

2. **全局选项和服务器信息：**

   ```
   ; (1 server found)
   ;; global options: +cmd
   ```

   - `(1 server found)`：表示找到了 1 个 DNS 服务器。
   - `global options: +cmd`：指明使用了全局选项 `+cmd`，该选项表示显示执行命令的指令。

3. **回答头部信息：**

   ```
   ;; Got answer:
   ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 17709
   ;; flags: qr rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 1
   ```

   - `opcode: QUERY`：表示查询操作。
   - `status: NOERROR`：表示查询状态为没有错误。
   - `id: 17709`：表示查询的唯一标识符。
   - `flags: qr rd ra`：表示一般查询、递归查询、和响应可用的标志。
   - `QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 1`：表示查询包含 1 个问题、3 个回答，没有权威和附加信息。

4. **OPT PSEUDOSECTION（可选部分）：**

   ```
   ;; OPT PSEUDOSECTION:
   ; EDNS: version: 0, flags:; udp: 512
   ```

   - `EDNS: version: 0, flags:; udp: 512`：表示使用 EDNS（Extension Mechanisms for DNS），版本为 0，无特殊标志，最大传输单元（udp）为 512 字节。

5. **问题部分：**

   ```
   ;; QUESTION SECTION:
   ;registry-1.docker.io.          IN      A
   ```

   - `registry-1.docker.io. IN A`：表示查询了 `registry-1.docker.io` 的A记录。

6. **回答部分：**

   ```
   ;; ANSWER SECTION:
   registry-1.docker.io.   54      IN      A       54.242.59.189
   registry-1.docker.io.   54      IN      A       3.215.51.67
   registry-1.docker.io.   54      IN      A       54.83.42.45
   ```

   - `registry-1.docker.io.`：表示回答的域名。
   - `54 IN A`：表示记录的生存时间（TTL）为 54 秒，记录类型为 A，即 IPv4 地址。
   - `54.242.59.189`、`3.215.51.67`、`54.83.42.45`：表示 `registry-1.docker.io` 的 IPv4 地址。

7. **其他信息：**

   ```
   ;; Query time: 16 msec
   ;; SERVER: 114.114.114.114#53(114.114.114.114)
   ;; WHEN: Mon Aug 29 14:20:08 CST 2022
   ;; MSG SIZE  rcvd: 97
   ```

   - `Query time: 16 msec`：表示查询耗时 16 毫秒。
   - `SERVER: 114.114.114.114#53(114.114.114.114)`：表示使用的 DNS 服务器及其地址。
   - `WHEN: Mon Aug 29 14:20:08 CST 2022`：表示查询的时间。
   - `MSG SIZE rcvd: 97`：表示接收到的消息大小为 97 字节。

#### 什么是 A 记录

A记录（Address Record）是 DNS（Domain Name System）中的一种资源记录类型，用于将主机名映射到 IPv4 地址。每个 A 记录包含一个主机名和对应的 IPv4 地址。当你在浏览器中输入一个域名（比如 www.example.com）时，系统首先会向 DNS 服务器发送一个 A 记录查询，以获取与该域名关联的 IPv4 地址。

例如，以下是一个 A 记录的简单示例：

```plaintext
example.com.    IN    A    192.168.1.1
```

- `example.com.` 是主机名。
- `IN` 表示 Internet。
- `A` 表示 A 记录类型。
- `192.168.1.1` 是与主机名相关联的 IPv4 地址。

当系统收到类似 www.example.com 的请求时，它会查询 DNS 服务器以获取相应的 A 记录，然后将解析得到的 IPv4 地址用于建立连接。

需要注意的是，A 记录只能映射到 IPv4 地址。对于 IPv6 地址的映射，可以使用 AAAA 记录。

-----------------------------------------

```
example.com.    IN    A    192.168.1.1
```

1. 第二列的内容是`IN`，表示 Internet。除了`IN`外，第二列还可能是其他一些区域（Zone）或者特定网络环境的标识，例如：

- `CH`：表示 CHAOS 类。
- `HS`：表示 Hesiod 类。
- 其他一些特定用途的标识。

但在实际应用中，`IN` 是最常见的，也是默认的区域标识，用于指定 Internet。

2. 第三列的内容是 `A`，表示 A 记录类型。除了 `A` 外，DNS 还支持其他类型的记录，每种记录类型都有特定的用途。一些常见的 DNS 记录类型包括：

- `AAAA`：IPv6 地址记录，类似于 A 记录，但用于映射主机名到 IPv6 地址。
- `CNAME`：规范名称记录，用于创建别名，将一个主机名映射到另一个主机名。
- `MX`：邮件交换记录，指定邮件服务器的优先级和域名。
- `NS`：域名服务器记录，指定域名的权威 DNS 服务器。
- `PTR`：指针记录，用于反向 DNS 查找，将 IP 地址映射到主机名。
- `SOA`：起始授权机构记录，包含有关域的重要信息，如域的管理者、域的刷新时间等。

在查询结果中，`A`记录指定了主机名与 IPv4 地址之间的映射关系。不同的记录类型用于支持不同的网络服务和功能。

## kdig

kdig 相比 dig，增加了 DoH、DoT、DoQ 等功能。

安装：

```sh
brew install knot

add-apt-repository ppa:cz.nic-labs/knot-dns-latest
apt update
apt install knot-dnsutils
```

例：

```sh
kdig @dns.google example.com +tls
kdig @dns.google example.com +https
kdig @dns.google example.com +quic
```

[man kdig](https://www.knot-dns.cz/docs/2.6/html/man_kdig.html)