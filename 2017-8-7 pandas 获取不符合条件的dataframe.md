---
title: 2017-8-7 pandas 获取不符合条件的dataframe
date: 2017-8-7
categories: [数据分析, Pandas]
tags: [Python, Pandas, Contains]
---
## search for “does-not-contain” on a dataframe in pandas
> 问题来源：做项目时，想拿到不符合条件的所有数据，比如：通话类型有好多种（主叫、被叫、呼转……），现在想分析所有非主叫数据，那么问题就来了。

<!--more-->
### 方法一：df[~df.col.str.contains(word)]
```python
>>> df = pd.DataFrame({"A": ["Hello", "this", "World", "apple"]})
>>> df.A.str.contains("Hello|World")
0     True
1    False
2     True
3    False
Name: A, dtype: bool
>>> ~df.A.str.contains("Hello|World")
0    False
1     True
2    False
3     True
Name: A, dtype: bool
>>> df[~df.A.str.contains("Hello|World")]
       A
1   this
3  apple

[2 rows x 1 columns]
```
> 注意：
> - 似乎 `df[~(df.A.str.contains("Hello") | (df.A.str.contains("World")))]`比上面使用正则，速度会快点
> - 获取“非”数据的条数：`(~df.col3.str.contains('u|z')).sum()`

### 方法二：
```python 
df[df["col"].str.contains('this'|'that')==False]
>>> df = pd.DataFrame({"A": ["Hello", "this", "World", "apple"]})
>>> df[df['A'].str.contains("Hello|World")==False]
       A
1   this
3  apple

# 多个条件情况下：
# df[df["col1"].str.contains('this|that')==False and df["col2"].str.contains('foo|bar')==True]
```
### 方法三：
```python
>>> df = pd.DataFrame({"A": ["Hello", "this", "World", "apple"]})
>>> df['A'].str.contains(r'^(?:(?!Hello|World).)*$')
0    False
1     True
2    False
3     True
Name: A, dtype: bool
>>> df[df['A'].str.contains(r'^(?:(?!Hello|World).)*$')]
       A
1   this
3  apple
```
