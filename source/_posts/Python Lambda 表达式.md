Python 的 `lambda` 表达式，简称匿名函数，是一种创建小型匿名函数的简洁方式。与常规的 `def` 关键字定义的函数不同，`lambda` 函数只包含一个表达式，表达式的结果即为函数的返回值。`lambda` 函数通常用于需要短期使用的小函数，尤其是在函数式编程中，比如在 `map`、`filter` 和 `reduce` 等函数中。

## 语法

```python
lambda para1, para2, ... : expression
```

- `lambda`：关键字，表示这是一个匿名函数。
- `para1, para2, ...`：函数的参数，可以有多个，用逗号分隔。
- `expression`：一个表达式，使用这些参数进行计算，并返回结果。

## 示例

### 基本示例

```python
# 定义一个 lambda 函数，计算两个数的和
sum_func = lambda x, y: x + y

# 调用 lambda 函数
result = sum_func(10, 20)
print(result)  # 30
```

### 在 `map`、`filter` 和 `reduce` 中使用

```python
# 使用 map 函数
numbers = [1, 2, 3, 4, 5]
squared = map(lambda x: x ** 2, numbers)
print(list(squared))  # [1, 4, 9, 16, 25]

# 使用 filter 函数
even_numbers = filter(lambda x: x % 2 == 0, numbers)
print(list(even_numbers))  # [2, 4]

# 使用 reduce 函数
from functools import reduce
sum_of_numbers = reduce(lambda x, y: x + y, numbers)
print(sum_of_numbers)  # 15
```

### 作为其他函数的参数

```python
# 定义一个高阶函数，接受一个函数和一个值作为参数
def high_order_func(func, value):
    return func(value)

# 使用 lambda 函数作为参数
result = high_order_func(lambda x: x ** 2, 5)
print(result)  # 25
```