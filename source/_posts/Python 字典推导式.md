字典推导式（Dictionary Comprehension）是 Python 中一种简洁且高效的创建字典的方式。它类似于列表推导式，但用于生成字典。通过字典推导式，可以从一个可迭代对象（如列表或元组）中创建字典，并且可以在创建过程中应用条件或变换。

### 基本语法

```python
my_dict = {key_expression: value_expression for item in iterable if condition}
```

有时也写作这种形式：

```python
my_dict = {
    key_expression: value_expression
    for item in iterable
    if condition
}
```

- `key_expression` 是新字典中的键表达式。
- `value_expression` 是新字典中的值表达式。
- `item` 是从 `iterable` 中迭代出来的元素。
- `iterable` 是一个可迭代对象。
- `condition` 是一个可选的条件，用于过滤字典项。

### 示例

1. 简单的字典推导式

   ```python
   # 创建一个字典，其中键是 1 到 5 的数字，值是这些数字的平方
   squares = {x: x**2 for x in range(1, 6)}
   print(squares)
   # 输出: {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
   ```

2. 带条件的字典推导式

   ```python
   # 创建一个字典，其中键是 1 到 10 的数字，值是这些数字的平方，但只包括偶数
   even_squares = {x: x**2 for x in range(1, 11) if x % 2 == 0}
   print(even_squares)
   # 输出: {2: 4, 4: 16, 6: 36, 8: 64, 10: 100}
   ```

3. 从两个列表创建字典

   ```python
   keys = ['a', 'b', 'c']
   values = [1, 2, 3]
   my_dict = {k: v for k, v in zip(keys, values)}
   print(my_dict)
   # 输出: {'a': 1, 'b': 2, 'c': 3}
   ```

4. 修改现有字典

   ```python
   # 将现有字典的值全部平方
   old_dict = {'a': 2, 'b': 3, 'c': 4}
   new_dict = {k: v**2 for k, v in old_dict.items()}
   print(new_dict)
   # 输出: {'a': 4, 'b': 9, 'c': 16}
   ```

5. 处理嵌套结构

   ```python
   # 创建一个字典，其中键是数字，值是这些数字的平方，并且值大于 10
   nested_dict = {x: x**2 for x in range(1, 11) if x**2 > 10}
   print(nested_dict)
   # 输出: {4: 16, 5: 25, 6: 36, 7: 49, 8: 64, 9: 81, 10: 100}
   ```
