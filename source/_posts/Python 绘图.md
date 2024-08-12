## 折线图

```py
import matplotlib.pyplot as plt
import matplotlib.font_manager as fm

# 设置字体族：英文使用 Times New Roman，中文使用 SimSun
plt.rcParams['font.family'] = ['Times New Roman', 'SimSun']

# 数据
x = [1, 2, 3, 4, 5]
y = [2, 3, 5, 7, 11]

# 创建绘图
plt.plot(x, y, marker='o', linestyle='-', color='b', label='Line 1')

# 添加标题和标签
plt.title('简单线图 Simple Line Plot')
plt.xlabel('X 轴 X Axis')
plt.ylabel('Y 轴 Y Axis')

# 显示图例
plt.legend()

# 显示图形
plt.show()
```

![image](https://img2024.cnblogs.com/blog/2778973/202406/2778973-20240607154605045-103811098.svg)

## 散点图

```py
import seaborn as sns
import matplotlib.pyplot as plt

# 设置字体族：英文使用 Times New Roman，中文使用 SimSun
plt.rcParams['font.family'] = ['Times New Roman', 'SimSun']

# 示例数据集
tips = sns.load_dataset("tips")

# 绘制散点图
sns.scatterplot(data=tips, x="total_bill", y="tip", hue="time")

# 添加标题
plt.title('小费和总账单的散点图 Scatter Plot of Total Bill vs Tip')

# 显示图形
plt.show()
```

![image](https://img2024.cnblogs.com/blog/2778973/202406/2778973-20240607154541163-1495190654.svg)

## 示例

### 折线图

```py
import numpy as np
import matplotlib.pyplot as plt

plt.rcParams['font.family'] = ['Times New Roman', 'SimSun'] # 设置字体族
plt.rcParams['axes.unicode_minus'] = False # 显示负号

x = np.array([3,5,7,9,11,13,15,17,19,21])

A = np.array([0.9708, 0.6429, 1, 0.8333, 0.8841, 0.5867, 0.9352, 0.8000, 0.9359, 0.9405])
B = np.array([0.9708, 0.6558, 1, 0.8095, 0.8913, 0.5950, 0.9352, 0.8000, 0.9359, 0.9419])
C = np.array([0.9657, 0.6688, 0.9855, 0.7881, 0.8667, 0.5952, 0.9361, 0.7848, 0.9244, 0.9221])
D = np.array([0.9664, 0.6701, 0.9884, 0.7929, 0.8790, 0.6072, 0.9352, 0.7920, 0.9170, 0.9254])

# label 在图示（legend）中显示。若为数学公式，则最好在字符串前后添加 $ 符号
# color: b:blue, g:green, r:red, c:cyan, m:magenta, y:yellow, k:black, w:white
# 线型：- -- -. : ,
# marker: . , o v < * + 1

plt.figure(figsize=(10,5))
plt.grid(linestyle="--") # 设置背景网格线为虚线
ax = plt.gca()

ax.spines['top'].set_visible(False) # 去掉上边框
ax.spines['right'].set_visible(False) # 去掉右边框

# A 图
plt.plot(x, A, color="black", label="A algorithm", linewidth=1.5)
# B 图
plt.plot(x, B, "k--", label="B algorithm", linewidth=1.5)
# C 图
plt.plot(x, C, color="red", label="C algorithm", linewidth=1.5)
# D 图
plt.plot(x, D, "r--", label="D algorithm", linewidth=1.5)

# 设置标题
plt.title("example", fontsize=12, fontweight='bold')

# 设置坐标轴刻度标识
x_ticks=['dataset1', 'dataset2', 'dataset3', 'dataset4', 'dataset5', ' dataset6', 'dataset7', 'dataset8', 'dataset9', 'dataset10'] # X 轴刻度的标识
plt.xticks(x, x_ticks, fontsize=12, fontweight='bold')
plt.yticks(fontsize=12, fontweight='bold')

# 设置坐标轴标签
plt.xlabel("Data sets", fontsize=13, fontweight='bold')
plt.ylabel("Accuracy", fontsize=13, fontweight='bold')

# 设置坐标轴范围
plt.xlim(3,21)
# plt.ylim(0.5,1)

# 显示图例
# plt.legend()
plt.legend(loc=0, numpoints=1)
leg = plt.gca().get_legend()
ltext = leg.get_texts()
plt.setp(ltext, fontsize=12, fontweight='bold') # 设置图例字体的大小和粗细

plt.savefig('filename.svg') # 建议保存为 svg 格式，再用 inkscape 转为矢量图 emf 后插入 word 中
plt.show()
```

![image](https://img2024.cnblogs.com/blog/2778973/202406/2778973-20240607155214676-1225919551.svg)

参考：[画论文折线图、曲线图？几个代码模板轻松搞定！](https://cloud.tencent.com/developer/article/1540478)