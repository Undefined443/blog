PHP（Hypertext Preprocessor）是一种广泛用于 Web 开发的服务器端脚本语言。它可以嵌入到 HTML 中，用于生成动态网页。

## 基本语法

PHP 代码通常嵌入在 HTML 中，使用 `<?php ... ?>` 标签包围。

```php
<!DOCTYPE html>
<html>
<head>
    <title>PHP 示例</title>
</head>
<body>
    <h1><?php echo "Hello, World!"; ?></h1>
</body>
</html>
```

### 变量与数据类型

PHP 变量以 `$` 开头，数据类型包括字符串、整数、浮点数、布尔值、数组和对象等。

```php
<?php
$name = "John";
$age = 30;
$isStudent = true;
$gpa = 3.75;

echo "Name: $name, Age: $age, Student: " . ($isStudent ? "Yes" : "No") . ", GPA: $gpa";
?>
```

### 数组

PHP 中的数组有两种类型：索引数组和关联数组。

```php
// 索引数组
$colors = array("Red", "Green", "Blue");
echo $colors[0]; // 输出: Red

// 关联数组
$person = array("name" => "John", "age" => 30, "city" => "New York");
echo $person["name"]; // 输出: John
```

### 条件语句

PHP 支持常见的条件语句如 `if`, `else`, `elseif` 和 `switch`。

```php
<?php
$grade = 85;

if ($grade >= 90) {
    echo "A";
} elseif ($grade >= 80) {
    echo "B";
} elseif ($grade >= 70) {
    echo "C";
} else {
    echo "F";
}
?>
```

### 循环

PHP 支持 `for`, `while`, `do-while` 和 `foreach` 循环。

```php
// for 循环
for ($i = 0; $i < 5; $i++) {
    echo "Number: $i <br>";
}

// while 循环
$j = 0;
while ($j < 5) {
    echo "Number: $j <br>";
    $j++;
}

// foreach 循环
$colors = array("Red", "Green", "Blue");
foreach ($colors as $color) {
    echo "Color: $color <br>";
}
```

### 函数

PHP 支持用户定义函数。

```php
<?php
function greet($name) {
    return "Hello, $name!";
}

echo greet("John");
?>
```

### 表单处理

PHP 可以处理 HTML 表单数据。

```php
<!-- HTML 表单 -->
<form method="post" action="<?php echo $_SERVER['PHP_SELF']; ?>">
    Name: <input type="text" name="name">
    <input type="submit">
</form>

<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    // 收集并处理表单数据
    $name = htmlspecialchars($_POST['name']);
    echo "Hello, $name!";
}
?>
```

### 数据库连接

PHP 常用的数据库是 MySQL，这里是一个简单的连接和查询示例。

```php
<?php
$servername = "localhost";
$username = "username";
$password = "password";
$dbname = "database";

// 创建连接
$conn = new mysqli($servername, $username, $password, $dbname);

// 检测连接
if ($conn->connect_error) {
    die("连接失败: " . $conn->connect_error);
}

// 查询数据库
$sql = "SELECT id, firstname, lastname FROM MyGuests";
$result = $conn->query($sql);

if ($result->num_rows > 0) {
    // 输出每行数据
    while($row = $result->fetch_assoc()) {
        echo "id: " . $row["id"]. " - Name: " . $row["firstname"]. " " . $row["lastname"]. "<br>";
    }
} else {
    echo "0 结果";
}

$conn->close();
?>
```

## 运行

1. 安装 Apache HTTP Server 和 PHP：

   ```sh
   brew install apache2 php
   ```

2. 配置 Apache HTTP Server：

   编辑 `/opt/homebrew/etc/httpd/httpd.conf`，添加以下内容：

   ```sh
   LoadModule php_module /opt/homebrew/opt/php/lib/httpd/modules/libphp.so

   <FilesMatch \.php$>
       SetHandler application/x-httpd-php
   </FilesMatch>
   ```

3. 在 DocumentRoot 创建一个 PHP 文件

4. 启动 Apache HTTP Server

5. 打开 http://127.0.0.1:8080/index.php 验证

参见：

- [PHP 手册](https://www.php.net/manual/zh/index.php)
- [XAMPP](https://www.apachefriends.org/index.html)