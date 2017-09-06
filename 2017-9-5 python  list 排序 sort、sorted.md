---
title: 2017-9-5 python  list 排序 sort、sorted 
date: 2017-9-5 15:17:50
categories: [Notes, Python]
tags: [Python, list, sort]
grammar_cjkRuby: true
---

### 情景描述：
> 项目中，有一个list，list内部组成元素为dict，现需要根据dict中某个键的值来进行排序。

<!-- more -->
### 代码实现：
>  `reverse`: False--默认，正序；True--逆序，由大到小；
>  `key`: 可以根据`key`值自定义排序；
>  `sort`与`sorted`区别: `sort`list自身发生改变；`sorted`list本身不发生改变。
```python
a = [5,2,1,9,6]        
 
 # sorted 用法
>>> sorted(a)                  #将a从小到大排序,不影响a本身结构 
[1, 2, 5, 6, 9] 
>>> sorted(a,reverse = True)   #将a从大到小排序,不影响a本身结构 
[9, 6, 5, 2, 1] 
 
 # sort 用法
>>> a.sort()                   #将a从小到大排序,影响a本身结构 
>>> a 
[1, 2, 5, 6, 9] 
>>> a.sort(reverse = True)     #将a从大到小排序,影响a本身结构 
>>> a 
[9, 6, 5, 2, 1] 
 
# 注意，a.sort() 已改变其结构，b = a.sort() 是错误的写法! 

# 非数字排序
>>> b = ['aa','BB','bb','zz','CC'] 
>>> sorted(b) 
['BB', 'CC', 'aa', 'bb', 'zz']    #按列表中元素每个字母的ascii码从小到大排序,如果要从大到小,请用sorted(b,reverse=True)下同 
 
 # 根据key值自定义排序
>>> c =['CCC', 'bb', 'ffff', 'z']  
>>> sorted(c,key=len)             #按列表的元素的长度排序 
['z', 'bb', 'CCC', 'ffff'] 
>>> d =['CCC', 'bb', 'ffff', 'z'] 
>>> sorted(d,key = str.lower )    #将列表中的每个元素变为小写，再按每个元素中的每个字母的ascii码从小到大排序 
['bb', 'CCC', 'ffff', 'z'] 
 
>>> def lastchar(s): 
       return s[-1] 
>>> e = ['abc','b','AAz','ef'] 
>>> sorted(e,key = lastchar)      #自定义函数排序,lastchar为函数名，这个函数返回列表e中每个元素的最后一个字母 
['b', 'abc', 'ef', 'AAz']         #sorted(e,key=lastchar)作用就是 按列表e中每个元素的最后一个字母的ascii码从小到大排序 
 
>>> f = [{'name':'abc','age':20},{'name':'def','age':30},{'name':'ghi','age':25}]     #列表中的元素为字典 
>>> def age(s): 
       return s['age'] 
>>> ff = sorted(f,key = age)      #自定义函数按列表f中字典的age从小到大排序  
[{'age': 20, 'name': 'abc'}, {'age': 25, 'name': 'ghi'}, {'age': 30, 'name': 'def'}] 
 
>>> f2 = sorted(f,key = lambda x:x['age'])    #如果觉得上面定义一个函数代码不美观，可以用lambda的形式来定义函数,效果[{'age': 20, 'name': 'abc'}, {'age': 25, 'name': 'ghi'}, {'age': 30, 'name': 'def'}] 
```

