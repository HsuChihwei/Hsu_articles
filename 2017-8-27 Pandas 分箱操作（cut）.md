---
title: 2017-8-27 Pandas 分箱操作（cut） 
date: 2017-8-27 11:12:47
categories: [数据分析, Pandas]
tags: [Python, Pandas, cut, 评分卡]
---
### 情景描述：
> 最新，项目中涉及到评分卡操作，评分项目有大概几十项，每项基本都是按频次区间给一个分数，最后，累计所有项目的分数得出最后所需要的分数。

<!--more-->
### demo1：
```python
import pandas as pd
import numpy as np
df = pandas.DataFrame({"a": np.random.random(100), "b": np.random.random(100), "id": np.arange(100)})

# Bin the data frame by "a" with 10 bins...
bins = np.linspace(df.a.min(), df.a.max(), 10)
# array([ 0.00282977,  0.11097259,  0.2191154 ,  0.32725822,  0.43540104,
        0.54354385,  0.65168667,  0.75982948,  0.8679723 ,  0.97611511])

# bins = np.linspace(0, 1, 11) # 优化版
# array([ 0. ,  0.1,  0.2,  0.3,  0.4,  0.5,  0.6,  0.7,  0.8,  0.9,  1. ])

groups = df.groupby(np.digitize(df.a, bins))

# Get the mean of each bin:
print groups.mean() # Also could do "groups.aggregate(np.mean)"
>>>output:
           a         b         id
1   0.044003  0.525964  56.307692
2   0.167568  0.506078  55.454545
3   0.268109  0.510612  44.636364
4   0.375014  0.544154  69.833333
5   0.481702  0.590031  48.500000
6   0.599587  0.488921  38.076923
7   0.696548  0.643555  50.642857
8   0.830064  0.620650  50.571429
9   0.928396  0.545460  44.166667
10  0.976115  0.693051  28.000000

# Similarly, the median:
print groups.median()
>>>output:
          a         b    id
1   0.028901  0.536857  61.0
2   0.167054  0.557716  49.0
3   0.267337  0.534911  43.0
4   0.374787  0.487063  73.0
5   0.480395  0.737007  49.5
6   0.603701  0.676479  42.0
7   0.695939  0.689144  57.5
8   0.836665  0.690757  41.0
9   0.924245  0.646487  47.0
10  0.976115  0.693051  28.0

# Apply some arbitrary function to aggregate binned data
print groups.aggregate(lambda x: np.mean(x[x > 0.5]))
>>>output:
           a         b         id
1        NaN  0.671236  56.307692
2        NaN  0.704379  55.454545
3        NaN  0.768609  44.636364
4        NaN  0.804354  69.833333
5   0.514166  0.796151  48.500000
6   0.599587  0.755381  41.250000
7   0.696548  0.779524  50.642857
8   0.830064  0.766095  50.571429
9   0.928396  0.902529  44.166667
10  0.976115  0.693051  28.000000
```

#### demo2 :
```python 
import numpy as np
import pandas

df = pandas.DataFrame({"a": np.random.random(100), 
                       "b": np.random.random(100) + 10})

# Bin the data frame by "a" with 10 bins...
bins = np.linspace(df.a.min(), df.a.max(), 10)
groups = df.groupby(pandas.cut(df.a, bins))

# Get the mean of b, binned by the values in a
print groups.mean().b
>>>output:
a
(0.000351, 0.11]    10.596542
(0.11, 0.22]        10.690010
(0.22, 0.33]        10.250080
(0.33, 0.44]        10.546134
(0.44, 0.549]       10.471454
(0.549, 0.659]      10.455624
(0.659, 0.769]      10.501616
(0.769, 0.879]      10.588354
(0.879, 0.989]      10.461848
Name: b, dtype: float64

```

#### demo3:
```python
In [144]: df = DataFrame({"a": np.random.random(100), "b": np.random.random(100), "id":   np.arange(100)})

In [145]: bins = [0, .25, .5, .75, 1]

In [146]: a_bins = df.a.groupby(cut(df.a,bins))

In [147]: b_bins = df.b.groupby(cut(df.b,bins))

In [148]: a_bins.agg([mean,median])
Out[148]:
                 mean    median
a
(0, 0.25]    0.124173  0.114613
(0.25, 0.5]  0.367703  0.358866
(0.5, 0.75]  0.624251  0.626730
(0.75, 1]    0.875395  0.869843

In [149]: b_bins.agg([mean,median])
Out[149]:
                 mean    median
b
(0, 0.25]    0.147936  0.166900
(0.25, 0.5]  0.394918  0.386729
(0.5, 0.75]  0.636111  0.655247
(0.75, 1]    0.851227  0.838805
```

#### demo4:
```python
import pandas as pd
import numpy as np

In [168]: filter_values = [0, 5, 17, 33]

In [169]: df = pd.DataFrame(np.random.random(100)*100, columns  = ['filtercol'])

In [170]: out = pd.cut(df.filtercol, bins = filter_values)

In [171]: counts = pd.value_counts(out)
Out[171]:
(17, 33]    16
(5, 17]     11
(0, 5]       5

# 排序
counts = counts.reindex(out.cat.categories)
counts = counts.sort_index()

In [172]: counts = counts.reindex(out.cat.categories)
In [173]: counts
Out[173]:
(0, 5]       5
(5, 17]     11
(17, 33]    16
Name: filtercol, dtype: int64

# 重置索引(reset index)
out = counts.reset_index(drop=True) # counts 不变
counts.reset_index(drop=True, inplace=True) # 直接改变counts

In [174]: out = counts.reset_index(drop=True)
In [175]: out
Out[175]:
0     5
1    11
2    16
Name: filtercol, dtype: int64

In [176]: counts
Out[176]:
(0, 5]       5
(5, 17]     11
(17, 33]    16
Name: filtercol, dtype: int64

In [177]: counts.reset_index(drop=True, inplace=True)
In [178]: counts
Out[178]:
0     5
1    11
2    16
Name: filtercol, dtype: int64
```

#### 总结：
> 第四个demo基本就可以完成当前目标了；
> 后续需要操作的是封装一个合适的通用方法，将每个项目评分标准代入即可。