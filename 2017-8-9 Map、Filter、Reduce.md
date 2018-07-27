---
title: Map、Filter、Reduce
date: 2017-8-9 
categories: [Notes, Python]
tags: [Python, Map, Filter, Reduce]
---

### Map
> 将一个函数映射到一个输入列表的所有元素上。

```python 
items = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, items))

# output:
[1, 4, 9, 16, 25]
```
<!--more-->
#### map作用于一列表的函数：
```python
def multiply(x):
	return (x*x)
def add(x):
	return (x+x)

funcs = [multiply, add]

for i in xrange(5):
    value = map(lambda x: x(i), funcs)
    print(list(value))

# Output:
# [0, 0]
# [1, 2]
# [4, 4]
# [9, 6]
# [16, 8]
```
 > 注：上面`print`加list转换，是为了python2/3的兼容，在python2中map直接返回列表，但在python3中返回迭代器。

### Filter：
> 过滤列表元素，返回符合要求的元素所组成的列表；
> filter类似for循环，但更快。
```python
number_list = range(-5, 5)
less_than_zero = filter(lambda x: x < 0, number_list)
print(list(less_than_zero))  

# Output: 
[-5, -4, -3, -2, -1]
```

### Reduce：
> 对列表进行计算并返回结果。
```python
# 计算列表乘积：
from functools import reduce
product = reduce( (lambda x, y: x * y), [1, 2, 3, 4] )

# Output: 
24

# 计算静默期：
count_times：func，计算静默时间
blank_ret：list
reduce(count_times, sorted(blank_ret))
```