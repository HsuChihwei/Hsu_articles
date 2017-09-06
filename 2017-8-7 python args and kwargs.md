---
title: 2017-8-7 python args and kwargs
date: 2017-8-7
categories: [Notes, Python]
tags: [Python, args, kwargs]
---
### `*args`：
> 传递一个非键值对的可变数量的参数列表给一个函数。
```python
def test_var_args(f_arg, *argv):
    print("first normal arg:", f_arg)
    for arg in argv:
        print("another arg through *argv:", arg)

test_var_args('yasoob', 'python', 'eggs', 'test')

# output：
# first normal arg: yasoob
# another arg through *argv: python
# another arg through *argv: eggs
# another arg through *argv: test
```
<!--more-->
### `**kwargs`：
> 传递不定长度的键值对（字典）, 作为参数传递给一个函数（即传递带名字的参数）。
```python
def greet_me(**kwargs):
    for key, value in kwargs.items():
        print("{0} == {1}".format(key, value))

>>> greet_me(name="yasoob")
name == yasoob
```

### 注意：
> 不是必须写成`*args`和`**kwargs`，只有变量前面的 `*`(星号)才是必须的；可以写成`*var` 和`**vars`，写成`*args` 和`**kwargs`只是一个通俗的命名约定；
> `*args`： 顺序不能变动；`**kwargs`：可以根据键指定顺序。
```python
def test_args_kwargs(arg1, arg2, arg3):
    print("arg1:", arg1)
    print("arg2:", arg2)
    print("arg3:", arg3)
	
# 首先使用 *args
>>> args = ("two", 3, 5)
>>> test_args_kwargs(*args)
arg1: two
arg2: 3
arg3: 5

# 现在使用 **kwargs:
>>> kwargs = {"arg3": 3, "arg2": "two", "arg1": 5}
>>> test_args_kwargs(**kwargs)
arg1: 5
arg2: two
arg3: 3
```
