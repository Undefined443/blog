需要安装 [Financial Toolbox](https://ww2.mathworks.cn/help/finance/index.html)。

```matlab
% 示例数据
openPrices  = [100, 102, 104, 103, 105];
highPrices  = [105, 107, 106, 108, 109];
lowPrices   = [99, 101, 103, 102, 104];
closePrices = [104, 105, 103, 107, 108];

data = [openPrices', highPrices', lowPrices', closePrices'];

% 创建一个新的图形窗口
figure;

% 绘制 K 线图
candlestick = candle(data);
dateaxis('x', 2, datetime(2023,1,1)) % 将序列-日期轴标签转换为日历-日期轴标签

% 设置图表属性
title('K Line Chart');
xlabel('Date');
ylabel('Price');
grid on;
```

![image](https://img2024.cnblogs.com/blog/2778973/202406/2778973-20240619014434634-187559868.svg)

参考：

- [使用自定义日期轴创建蜡烛图 | MathWorks](https://ww2.mathworks.cn/help/thingspeak/candle-plot-with-customized-date-axis.html)
- [candle | MathWorks](https://ww2.mathworks.cn/help/finance/candle.html)
- [dateaxis | MathWorks](https://ww2.mathworks.cn/help/finance/dateaxis.html)