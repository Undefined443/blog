### 数字

```py
n: int = 1000000000
print(f'{n:_}')  # 1_000_000_000
print(f'{n:,}')  # 1,000,000,000
```

### 对齐

```py
var: str = 'var'
# 右对齐，使用 _ 填充
print(f'{var:_>20}')  # _________________var
# 左对齐，使用 # 填充
print(f'{var:#<20}')  # var#################
# 居中，使用 | 填充|
print(f'{var:|^20}')  # ||||||||var|||||||||
```

### 日期

```py
from datetime import datetime

now: datetime = datetime.now()
# 日.月.年 (时:分:秒)
print(f'{now:%d.%m.%y (%H:%M:%S)}')  # 25.02.24 (01:08:19)
print(f'{now:%c}')  # Sun Feb 25 01:08:19 2024
print(f'{now:%I%p}')  # 01AM
```

### 浮点数

```py
n: float = 1234.5678
print(f'Result: {n:.2f}')  # Result: 1234.57
print(f'Result: {n:.0f}')  # Result: 1235
print(f'Result: {n:_.3f}')  # Result: 1_234.568
```

### 自动运算

```py
my_var: str = 'Bob says hi'
print(f'{my_var = }')  # my_var = 'Bob says hi'

a = 1
b = 2
print(f'{a + b = }')  # a + b = 3
```