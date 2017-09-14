---
title: 2017-8-28 python interval  区间处理
date: 2017-8-28 18:25:8
categories: [Notes, Python]
tags: [Python, 区间, interval]
---
```python
>>> volume1 = Interval.between("A", "Foe")
>>> volume2 = Interval.between("Fog", "McAfee")
>>> volume3 = Interval.between("McDonalds", "Space")
>>> volume4 = Interval.between("Spade", "Zygote")
>>> encyclopedia = IntervalSet([volume1, volume2, volume3, volume4])
>>> mySet = IntervalSet([volume1, volume3, volume4])
>>> "Meteor" in encyclopedia
True
>>> "Goose" in encyclopedia
True
>>> "Goose" in mySet
False
>>> volume2 in (encyclopedia ^ mySet)
True
```
```python
a= 112 
In [4]:  a in range(300,400) 
   ...: 
Out[4]: False 
In [5]:  a in range(101,300) 
   ...: 
Out[5]: True 
```
```python
python强大的区间处理库interval用法介绍
原文发表在我的博客主页，转载请注明出处

前言

这个库是在阅读别人的源码的时候看到的，觉得十分好用，然而在网上找到的相关资料甚少，所以阅读了源码来做一个简单的用法总结。在网络的路由表中，经常会通过掩码来表示流表的匹配域，在python中有的时候为了方便的模拟流表的匹配过程，可以通过一个整数区间来表示诸如IP等的匹配范围，而本文介绍的库在区间处理上是十分的强大与方便。

用法举例

不论是在Linux系统还是Windows系统上，我们都可以方便的安装pip或者easy_install库来方便的安装大多数python库，interval也不例外。
在这个库中提供了两个主要的类，分别是Interval和IntervalSet两个类。
Interval类描述了一个连续的范围区间，这个区间可以是闭、开、半闭半开、无穷的，他的区间值不一定是数字，可以包含任何的数据类型，比如字符串，时间等等，同时他和python的各种操作（<, <=, ==, >=, >等）也是兼容的。IntervalSet包含了一个或多个互不相交的Interval集合。下面的这几个例子是源码中的。

 >>>volume1 = Interval.between("A", "Foe")
>>>volume2 = Interval.between("Fog", "McAfee")
>>>volume3 = Interval.between("McDonalds", "Space")
>>>volume4 = Interval.between("Spade", "Zygote")
>>>encyclopedia = IntervalSet([volume1, volume2, volume3, volume4])
>>>mySet = IntervalSet([volume1, volume3, volume4])
>>>"Meteor" in encyclopedia
True
>>>"Goose" in encyclopedia
True
>>>"Goose" in mySet
False
>>>volume2 in (encyclopedia ^ mySet)
True
前面的三个例子比较容易理解，最后一个例子中，encyclopedia的区别就是mySet多了一个volume2，而异或就是将两个集合中相同的元素去掉，不同的元素保留，所以最后只剩下了volume2。
除了字符串，利用interval还可以很方便的处理时间，下面的例子同样来自于源码。

 >>>officeHours = IntervalSet.between("08:00", "17:00")
>>>myLunch = IntervalSet.between("11:30", "12:30")
>>>myHours = IntervalSet.between("08:30", "19:30") - myLunch
>>>myHours.issubset(officeHours)
False
>>>"12:00" in myHours
False
>>>"15:30" in myHours
True
>>>inOffice = officeHours & myHours
>>>print inOffice
['08:30'..'11:30'),('12:30'..'17:00']
>>>overtime = myHours - officeHours
>>>print overtime
('17:00'..'19:30']
在前言中说道interval库可以处理IP地址，简单的列举应用如下：

 # coding
r1 = IntervalSet([Interval(1, 1000), Interval(1100, 1200)])
r2 = IntervalSet([Interval(30, 50), Interval(60, 200), Interval(1150, 1300)])

r3 = IntervalSet([Interval(1000, 3000)])
r4 = IntervalSet([Interval(1000, 3000)])
r5 = IntervalSet([Interval(30000, 12000)])

print (r3 - r4), (r4 - r3), r3 & r4
print len(IntervalSet.empty())

if r3 & r4 == r4:
    print 'yes'

print r3 & r4
if (r3 - r4).empty():
   print "true"
print (r3 - r4).empty()

# output
<Empty> <Empty> [1000..3000]
0
yes
[1000..3000]
<Empty>
常用方法

interval对象初始化参数（lower_bound=-Inf, upper_bound=Inf, **kwargs）三个boolean参数closed,lower_closed,upper_closed分表表示全闭，左闭右开，左开右闭。比如：r = Interval(upper_bound=62, closed=False)
between(a, b, closed=True)：返回以a和b为界的区间
less_than(a)：小于a的所有值构成interval，类似的还有less_than_or_equal_to，greater_than，greater_than_or_equal_to函数
join(other)：将两个连续的intervals组合起来
overlaps(other)：两个区间是否有重叠
adjacent_to(other)：两个区间是否不重叠的毗邻
总结

是一篇总结文章，并没有什么深度，只是为了不再重复造轮子，在必要的时候一个库可以极大的提高效率。
MeasureMeasure
```