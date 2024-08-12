Perl 是一种高效、功能强大且灵活的编程语言，广泛用于文本处理、系统管理、网络编程、Web 开发等领域。它由 Larry Wall 在 1987 年首次发布，名字来源于“Practical Extraction and Report Language”。

## Hello World

```perl
#!/usr/bin/perl
# 提高 Perl 脚本的健壮性和可维护性
use strict;
use warnings;

print "Hello, World!\n";
```

## 基本语法

### 变量

Perl 有三种主要类型的变量：标量、数组和哈希。

- **标量**：以 `$` 开头，用于存储单个值（字符串、整数、浮点数等）。

  ```perl
  my $name = "John";
  my $age = 30;
  ```

  > Perl 同时支持词法作用域（通过 my 声明）和动态作用域（通过 local 声明）。我们平时使用的是词法作用域。

- **数组**：以 `@` 开头，用于存储一组有序的值。

  ```perl
  my @colors = ("Red", "Green", "Blue");
  print $colors[0]; # 输出: Red
  ```

- **哈希**：以 `%` 开头，用于存储键值对。

  ```perl
  my %person = ("name" => "John", "age" => 30);
  print $person{"name"}; # 输出: John
  ```

### 条件语句

Perl 支持常见的条件语句如 `if`, `elsif`, `else` 和 `unless`。

```perl
my $grade = 85;

if ($grade >= 90) {
    print "A\n";
} elsif ($grade >= 80) {
    print "B\n";
} elsif ($grade >= 70) {
    print "C\n";
} else {
    print "F\n";
}
```

### 循环

Perl 支持 `for`, `foreach`, `while` 和 `until`循环。

```perl
# for 循环
for (my $i = 0; $i < 5; $i++) {
    print "Number: $i\n";
}

# while 循环
my $j = 0;
while ($j < 5) {
    print "Number: $j\n";
    $j++;
}

# foreach 循环
my @colors = ("Red", "Green", "Blue");
foreach my $color (@colors) {
    print "Color: $color\n";
}
```

### 函数

Perl 支持用户定义函数，使用 `sub` 关键字。

```perl
sub greet {
    my ($name) = @_;
    return "Hello, $name!";
}

print greet("John");
```

### 文件操作

Perl 提供了丰富的文件操作功能。

```perl
# 打开文件
open(my $fh, '<', 'file.txt') or die "不能打开文件: $!";

# 读取文件内容
while (my $line = <$fh>) {
    print $line;
}

# 关闭文件
close($fh);
```

### 模块和库

Perl 有一个庞大的模块和库生态系统，通过 CPAN 可以轻松安装和使用各种模块。

```sh
cpan install JSON # 安装模块
```

使用模块：

```perl
use JSON;

my $json_text = '{"name": "John", "age": 30}';
my $data = decode_json($json_text);

print $data->{name}; # 输出: John
```

## 运行

保存你的 Perl 脚本为`script.pl`，在命令行运行：

```sh
perl script.pl
```