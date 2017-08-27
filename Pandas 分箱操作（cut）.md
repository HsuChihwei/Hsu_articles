---
title: Pandas 分箱操作（cut） 
date: 2017-8-27 11:12:47
categories: Python, Notes
tags: Python, Pandas
grammar_cjkRuby: true
---
### 情景描述：
> 最新，项目中涉及到评分卡操作，评分项目有大概几十项，每项基本都是按频次区间给一个分数，最后，累计所有项目的分数得出最后所需要的分数。

### demo1：
```python
import pandas as pd
import numpy as np
df = pandas.DataFrame({"a": np.random.random(100), "b": np.random.random(100), "id": np.arange(100)})

bins = np.linspace(0, 1, 11)
# array([ 0. ,  0.1,  0.2,  0.3,  0.4,  0.5,  0.6,  0.7,  0.8,  0.9,  1. ])

# Bin the data frame by "a" with 10 bins...
bins = np.linspace(df.a.min(), df.a.max(), 10)
groups = df.groupby(np.digitize(df.a, bins))

# Get the mean of each bin:
print groups.mean() # Also could do "groups.aggregate(np.mean)"

# Similarly, the median:
print groups.median()

# Apply some arbitrary function to aggregate binned data
print groups.aggregate(lambda x: np.mean(x[x > 0.5]))

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

df = pd.DataFrame(range(50), columns  = ['filtercol'])
w = 0
x = 5
y = 17
z = 33
filter_values = [w, x, y, z]
out = pd.cut(df.filtercol, bins = filter_values)
counts = pd.value_counts(out)
# counts is a Series
print(counts)

# (17, 33]    16
# (5, 17]     12
# (0, 5]       5

# 排序
counts.reindex(out.cat.categories)
counts.sort_index()

# (0, 5]       5
# (5, 17]     12
# (17, 33]    16

# reset index
out = counts.reset_index(drop=True) # counts 不变
counts.reset_index(drop=True, inplace=True) # 直接改变counts


```