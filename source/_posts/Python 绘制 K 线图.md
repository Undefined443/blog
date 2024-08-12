```python
import mplfinance as mpf
import pandas as pd

# 示例数据
data = pd.DataFrame({
    'Date':  ['2023-01-01', '2023-01-02', '2023-01-03', '2023-01-04', '2023-01-05'],
    'Open':  [100, 102, 104, 103, 105],
    'High':  [105, 107, 106, 108, 109],
    'Low':   [99, 101, 103, 102, 104],
    'Close': [104, 105, 103, 107, 108]
})

# 将日期转换为 DatetimeIndex 格式
data.index = pd.DatetimeIndex(data['Date'])

# 绘制 K 线图
mpf.plot(data,type='candle')
```

![image](https://img2024.cnblogs.com/blog/2778973/202406/2778973-20240619022209708-784845366.svg)

也可以提供 `style` 命名参数来改变 K 线图的风格：

```python
mpf.plot(data,style='yahoo',type='candle')
```

![image](https://img2024.cnblogs.com/blog/2778973/202406/2778973-20240619022405021-780736598.svg)

参考：

- [matplotlib](https://github.com/matplotlib/mplfinance#tutorials)
- [pandas documentation](https://pandas.pydata.org/pandas-docs/stable/)

Dependencies:

```
mplfinance==0.12.10b0
pandas==2.2.2
```