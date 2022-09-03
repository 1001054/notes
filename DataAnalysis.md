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

x = range(2, 26, 2)

y = [13, 12, 14, 17, 20, 25, 26, 24, 22, 21, 16.5, 13]

plt.plot(x, y)
plt.show()

```

