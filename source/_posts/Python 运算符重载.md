在Python中，运算符重载是一种允许你定义或修改内置运算符（例如 `+`, `-`, `*`, `/` 等）在自定义类中的行为的技术。通过重载运算符，你可以使这些运算符对自定义对象执行特定的操作。运算符重载是通过在类中定义特殊方法（也称为魔法方法）来实现的，这些方法通常以双下划线开头和结尾。

以下是一些常见运算符及其对应的魔法方法：

1. **算术运算符**：
   - `+` (加法): `__add__(self, other)`
   - `-` (减法): `__sub__(self, other)`
   - `*` (乘法): `__mul__(self, other)`
   - `/` (除法): `__truediv__(self, other)`
   - `//` (整除): `__floordiv__(self, other)`
   - `%` (取模): `__mod__(self, other)`
   - `**` (幂): `__pow__(self, other)`

2. **比较运算符**：
   - `==` (等于): `__eq__(self, other)`
   - `!=` (不等于): `__ne__(self, other)`
   - `<` (小于): `__lt__(self, other)`
   - `<=` (小于等于): `__le__(self, other)`
   - `>` (大于): `__gt__(self, other)`
   - `>=` (大于等于): `__ge__(self, other)`

3. **位运算符**：
   - `&` (按位与): `__and__(self, other)`
   - `|` (按位或): `__or__(self, other)`
   - `^` (按位异或): `__xor__(self, other)`
   - `~` (按位取反): `__invert__(self)`
   - `<<` (左移): `__lshift__(self, other)`
   - `>>` (右移): `__rshift__(self, other)`

4. **其他运算符**：
   - `[]` (索引): `__getitem__(self, key)`
   - `in` (成员关系): `__contains__(self, item)`
   - `len()` (长度): `__len__(self)`

下面是一个简单的示例，展示如何重载加法运算符 `+` 和比较运算符 `==`：

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

    def __eq__(self, other):
        return self.x == other.x and self.y == other.y

    def __repr__(self):
        return f"Vector({self.x}, {self.y})"

# 示例用法
v1 = Vector(2, 3)
v2 = Vector(5, 7)
v3 = v1 + v2
print(v3)  # 输出: Vector(7, 10)

print(v1 == v2)  # 输出: False
print(v1 == Vector(2, 3))  # 输出: True
```

在这个示例中：
- `__add__` 方法定义了如何将两个 `Vector` 对象相加。
- `__eq__` 方法定义了如何比较两个 `Vector` 对象是否相等。
- `__repr__` 方法定义了当打印 `Vector` 对象时的输出格式。