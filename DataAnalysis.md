### 数据分析的流程

1. 提出问题
2. 准备数据
3. 分析数据
4. 获得结论
5. 成果可视化

## matplotlib

### pycharm配置

File -> settings -> projects -> python interpreter -> + ->"matplotlib"

### 气温示例

```python

from matplotlib import pyplot as plt
import random

x = range(1, 121)
y = [random.randint(20, 35) for i in range(120)]

# 设置图片大小和清晰度
plt.figure(figsize=(20, 8), dpi=80)
# 画图
plt.plot(x, y)
# 添加x轴刻度
plt.xticks(x[::5])
# 添加y轴刻度
plt.yticks(range(min(y) - 1, max(y) + 1))
# 保存
# plt.savefig("./t1.png")
# 显示图片
plt.show()
```

