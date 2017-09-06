---
title: 2017-8-27 dataframe与序列相乘 
date: 2017-8-27 18:28:54
categories: [数据分析, Pandas]
tags: [Python, Pandas, DataFrame]
---
### 情景描述：
> 继续完成项目中的评分卡部分，拿到分好箱的数据后，如何进行加权得到最后的分数就是我们接下来要考虑的问题了。

<!--more-->
#### demo1:
```python
result = dataframe.mul(series, axis=0)
```
#### demo2:
```python 
def tile_df(df, n, m):
 dfn = df.T
 for _ in range(1, m):
 dfn = dfn.append(df.T, ignore_index=True)
 dfm = dfn.T
 for _ in range(1, n):
 dfm = dfm.append(dfn.T, ignore_index=True)
 return dfm
 
df = pandas.DataFrame([[1,2],[3,4]])
tile_df(df, 2, 3)
# 0 1 2 3 4 5
# 0 1 2 1 2 1 2
# 1 3 4 3 4 3 4
# 2 1 2 1 2 1 2
# 3 3 4 3 4 3 4
```
#### demo3:
```python
>>> df = pd.DataFrame(np.random.randint(1, 10, (5, 3)))
>>> df
   0  1  2
0  7  7  5
1  1  8  6
2  4  8  4
3  2  9  5
4  3  8  7
>>> df.prod(axis=1)
0    245
1     48
2    128
3     90
4    168
dtype: int64

>>> df = pd.DataFrame(np.random.randint(1, 10, (5, 3)))
>>> df
   0  1  2
0  9  3  3
1  8  5  4
2  3  6  7
3  9  8  5
4  7  1  2
>>> df.apply(np.prod, axis=1)
0     81
1    160
2    126
3    360
4     14
dtype: int64

```
#### demo4:
```python
In[197]: import pandas as pd; import numpy as np
 
In [198]: df = pd.DataFrame(np.arange(40.).reshape((8, 5)), columns=list('abcde'));

In [199]: df
Out[199]:
      a     b     c     d     e
0   0.0   1.0   2.0   3.0   4.0
1   5.0   6.0   7.0   8.0   9.0
2  10.0  11.0  12.0  13.0  14.0
3  15.0  16.0  17.0  18.0  19.0
4  20.0  21.0  22.0  23.0  24.0
5  25.0  26.0  27.0  28.0  29.0
6  30.0  31.0  32.0  33.0  34.0
7  35.0  36.0  37.0  38.0  39.0

In [200]: ser = pd.Series(np.arange(8) * 10);

In [201]: ser
Out[201]:
0     0
1    10
2    20
3    30
4    40
5    50
6    60
7    70
dtype: int64


In [202]: func = lambda x: np.asarray(x) * np.asarray(ser)

In [203]: df.apply(func)
Out[203]:
        a       b       c       d       e
0     0.0     0.0     0.0     0.0     0.0
1    50.0    60.0    70.0    80.0    90.0
2   200.0   220.0   240.0   260.0   280.0
3   450.0   480.0   510.0   540.0   570.0
4   800.0   840.0   880.0   920.0   960.0
5  1250.0  1300.0  1350.0  1400.0  1450.0
6  1800.0  1860.0  1920.0  1980.0  2040.0
7  2450.0  2520.0  2590.0  2660.0  2730.0


In [204]: df.apply(func).a
Out[204]:
0       0.0
1      50.0
2     200.0
3     450.0
4     800.0
5    1250.0
6    1800.0
7    2450.0
Name: a, dtype: float64


# 行相乘
In[205]: ser2 = pd.Series(np.arange(5) *5); 

In [206]: ser2
Out[206]: 
 0 0
 1 5
 2 10
 3 15
 4 20

In[207]: func2 = lambda x: np.asarray(x) * np.asarray(ser2)

In[8]: df.apply(func2, axis=1)
Out[208]: 
 a b c d e
 0 0 5 20 45 80
 1 0 30 70 120 180
 2 0 55 120 195 280
 3 0 80 170 270 380
 4 0 105 220 345 480
 5 0 130 270 420 580
 6 0 155 320 495 680
 7 0 180 370 570 780
 
 # 进阶版
In[209]: df.apply(lambda x: np.asarray(x) * np.asarray(ser))
Out[209]: 
 a b c d e
 0 0 0 0 0 0
 1 50 60 70 80 90
 2 200 220 240 260 280
 3 450 480 510 540 570
 4 800 840 880 920 960
 5 1250 1300 1350 1400 1450
 6 1800 1860 1920 1980 2040
 7 2450 2520 2590 2660 2730
 
In [210]: df.apply(lambda x: np.asarray(x) * np.asarray(ser)).a
Out[210]:
0       0.0
1      50.0
2     200.0
3     450.0
4     800.0
5    1250.0
6    1800.0
7    2450.0
Name: a, dtype: float64

In[211]: df.apply(lambda x: np.asarray(x) * np.asarray(ser2), axis=1)
Out[211]:
 a b c d e
 0 0 5 20 45 80
 1 0 30 70 120 180
 2 0 55 120 195 280
 3 0 80 170 270 380
 4 0 105 220 345 480
 5 0 130 270 420 580
 6 0 155 320 495 680
 7 0 180 370 570 780
```

### 总结：
> 这样，我们总分也就拿到了，最后只需将每个项目的总分求和即可