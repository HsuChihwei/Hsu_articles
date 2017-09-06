---
title: 2017-7-31 python 生成器（Generators）
date: 2017-7-31
categories: [Notes, Python]
tags: [Python, generators]
---
### 可迭代对象（iterable）：
> 能提供迭代器的任意对象；只要定义了一个迭代器的__iter__方法或定义了支持下标索引的__getitem__方法，那就是一个可迭代对象。

### 迭代器（iterators）：
> 任意对象，只要定义了next或者__next__方法，那就是一个迭代器

### 迭代（iteration）：
> 从某个地方（如列表）取出一个元素的过程；使用一个循环来遍历某个东西（如列表），这个过程就是迭代；

<!--more-->
### 生成器（Generators）：
> 也是一种迭代器，但只能对其迭代一次
 - 因为它们并没有把所有值存在内存中，而是运行时生成值；
 - 可通过遍历使用它们，要么使用“for”循环，要么传递给任意可以进行迭代的函数和结构；
 - 大多时候生成器是以函数实现的，但它们并不返回一个值，而是yield（生出）一个值。
```python
def generator_function():
    for i in range(10):
        yield i

for item in generator_function():
    print(item)

# Output: 0
# 1
# 2
# 3
# 4
# 5
# 6
# 7
# 8
# 9
```
#### 适用场景：
> 不想同一时间将所有计算出来的大量结果集分配到内存当中，特别是结果集里还包含循环；因为这样做会消耗大量资源 
> 许多Python 2里的标准库函数都会返回列表，而Python 3都修改成了返回生成器，因为生成器占用更少的资源。

**斐波那契数列的生成器：

```python
# 传统模式
def fibon(n):
    a = b = 1
    result = []
    for i in range(n):
        result.append(a)
        a, b = b, a + b
    return result

# 生成器模式
def fibon(n):
    a = b = 1
    for i in range(n):
        yield a
        a, b = b, a + b

# Now we can use it like this:
for x in fibon(1000000):
    print(x)
```
### Python内置函数：`next()`
> 它允许我们获取一个序列的下一个元素
> 特点：
> - 在`yield`掉所有的值后，`next()`会触发一个`StopIteration`的异常。提示我们所有的值都已经被`yield`完了；
> - 在使用`for`循环时没有这个异常，因为`for`循环会自动捕捉到这个异常并停止调用`next()`。
```python
def generator_function():
    for i in range(3):
        yield i

gen = generator_function()
print(next(gen))
# Output: 0
print(next(gen))
# Output: 1
print(next(gen))
# Output: 2
print(next(gen))
# Output: Traceback (most recent call last):
#         File "<stdin>", line 1, in <module>
#         StopIteration
```


### Python内置函数：`iter()`
> 将一个可迭代对象返回一个迭代器对象
```python
my_string = "Bingo"
next(my_string)
# Output: Traceback (most recent call last):
#      File "<stdin>", line 1, in <module>
#    TypeError: str object is not an iterator
```
这个异常说`str`对象是一个可迭代对象，而不是一个迭代器；不能直接对其进行迭代操作，所以，使用`iter`：
```python
my_string = "Bingo"
my_iter = iter(my_string)
next(my_iter)
# Output: 'B'
next(my_iter)
# Output: 'i'
next(my_iter)
# Output: 'n'
next(my_iter)
# Output: 'g'
next(my_iter)
# Output: 'o'
next(my_iter)
# Output: Traceback (most recent call last):
#         File "<stdin>", line 12, in <module>
#         StopIteration
```
