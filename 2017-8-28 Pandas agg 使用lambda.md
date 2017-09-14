---
title: 2017-8-28 Pandas agg 使用lambda  
date: 2017-8-28 18:12:25
categories: [数据分析, Pandas]
tags: [Python, lambda, agg, Pandas]
---
```python
import numpy as np
import pandas as pd

N = 100
data = pd.DataFrame({
    'type': np.random.randint(10, size=N),
    'status': np.random.randint(10, size=N),
    'name': np.random.randint(10, size=N),
    'value': np.random.randint(10, size=N),
})

reading = np.random.random(10,)

data = data.groupby(['type', 'status', 'name'])['value'].agg({
    'one' : np.mean, 
    'two' : lambda value: 100* ((value>32).sum() / reading.mean()), 
    'test2': lambda value: 100* ((value > 45).sum() / value.mean())
})
print(data)
```
### 获取一列数据中最大值
```python 
In [34]: df.loc[df['Value'].idxmax()]
Out[34]: 
Country        US
Place      Kansas
Value         894
Name: 7

df = df.reset_index()

data.groupby(['Country','Place'])['Value'].max().item()

df.groupby(['country','place'], as_index=False)['value'].max()

df.groupby("country").apply(lambda df:df.irow(df.value.argmax()))

In [5]: df = pandas.DataFrame(np.random.randn(10,3),columns=['A','B','C'])

In [6]: df
Out[6]: 
          A         B         C
0  2.001289  0.482561  1.579985
1 -0.991646 -0.387835  1.320236
2  0.143826 -1.096889  1.486508
3 -0.193056 -0.499020  1.536540
4 -2.083647 -3.074591  0.175772
5 -0.186138 -1.949731  0.287432
6 -0.480790 -1.771560 -0.930234
7  0.227383 -0.278253  2.102004
8 -0.002592  1.434192 -1.624915
9  0.404911 -2.167599 -0.452900

In [7]: df.idxmax()
Out[7]: 
A    0
B    8
C    7

In [8]: df.ix[df['A'].idxmax()]
Out[8]: 
A    2.001289
B    0.482561
C    1.579985

···
