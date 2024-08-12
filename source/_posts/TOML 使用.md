[TOML Reference](https://toml.io/cn/v1.0.0)

TOML
: Tom's Obvious, Minimal Language

TOML 被设计成可以无歧义地映射为哈希表。（相当于 JSON 对象吧）

## 注释

```toml
# 这是一个全行注释
key = "value"  # 这是一个行末注释
another = "# 这不是一个注释"
```

## 键值对

TOML 文档最基本的构成区块是键值对。

```toml
name = "Orange"
physical.color = "orange"
physical.shape = "round"
site."google.com" = true
```

等价于 JSON 的如下结构：

```json
{
  "name": "Orange",
  "physical": {
    "color": "orange",
    "shape": "round"
  },
  "site": {
    "google.com": true
  }
}
```

## 字符串

多行基本字符串由三个引号包裹，允许折行。

- 紧随开头引号的那个换行会被去除
- 其它空白和换行会被原样保留

```toml
str1 = """
Roses are red
Violets are blue"""
```

## 数组

数组是内含值的方括号。

- 空白会被忽略
- 子元素由逗号分隔
- 数组可以包含与键值对所允许的相同数据类型的值
- 可以混合不同类型的值

```toml
integers = [ 1, 2, 3 ]
colors = [ "红", "黄", "绿" ]
nested_array_of_ints = [ [ 1, 2 ], [3, 4, 5] ]
nested_mixed_array = [ [ 1, 2 ], ["a", "b", "c"] ]
string_array = [ "所有的", '字符串', """是相同的""", '''类型''' ]

# 允许混合类型的数组
numbers = [ 0.1, 0.2, 0.5, 1, 2, 5 ]
contributors = [
  "Foo Bar <foo@example.com>",
  { name = "Baz Qux", email = "bazqux@example.com", url = "https://example.com/bazqux" }
]
```

- 数组可以跨行
- 数组的最后一个值后面可以有终逗号（也称为尾逗号）
- 值、逗号、结束括号前可以存在任意数量的换行和注释
- 数组值和逗号之间的缩进被作为空白对待而被忽略

```toml
integers2 = [
  1, 2, 3
]

integers3 = [
  1,
  2, # 这是可以的
]
```

## 表

表（也被称为哈希表或字典）是键值对的集合。

- 它们由表头定义，连同方括号作为单独的行出现
- 看得出表头不同于数组，因为数组只有值
- 在它下方，直至下一个表头或文件结束，都是这个表的键值对
- 表不保证保持键值对的指定顺序

```toml
[table-1]
key1 = "some string"
key2 = 123

[table-2]
key1 = "another string"
key2 = 456
```

等价于：

```json
{
  "table-1": {
    "key1": "some string",
    "key2": 123
  },
  "table-2": {
    "key1": "another string",
    "key2": 456
  }
}
```

## 表数组

这可以通过把表名写在双方括号里的表头来表示

- 表头的第一例定义了这个数组及其首个表元素，而后续的每个则在该数组中创建并定义一个新的表元素
- 这些表按出现顺序插入该数组

```toml
[[products]]
name = "Hammer"
sku = 738594937

[[products]]  # 数组里的空表

[[products]]
name = "Nail"
sku = 284758393

color = "gray"
```

等价于：

```json
{
  "products": [
    { "name": "Hammer", "sku": 738594937 },
    { },
    { "name": "Nail", "sku": 284758393, "color": "gray" }
  ]
}
```