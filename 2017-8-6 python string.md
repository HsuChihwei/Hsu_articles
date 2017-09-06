---
title: 2017-8-6 python string 
date: 2017-8-6
categories: [Notes, Python]
tags: [Python, string]
---
## The most pythonic way to pad zeroes to string

### Strings:
```python
>>> n = '4'
>>> n.zfill(3)
>>> '004'

>>> '{:0>3}'.format(n)
>>> '004'
>>> '{1}{1}{0}'.format(n, '0')
>>> '004'
>>> '{:0<3}'.format(n)
>>> '400'
>>> '{:-^11}'.format(n)
>>> '-----4-----'

>>> n.rjust(3, '0')
>>> '004'
>>> n.ljust(3, '0')
>>> '400'
>>> '4'.center(11,"-")
>>> '-----4-----'
```
<!--more-->
`rjust/zfill` 区别:
```python
zfill:
>>> '--txt'.zfill(10)
>>> '-00000-txt'
>>> '++txt'.zfill(10)
>>> '+00000+txt'
>>> '..txt'.zfill(10)
>>> '00000..txt'
rjust:
>>> '--txt'.rjust(10, '0')
>>> '00000--txt'
>>> '++txt'.rjust(10, '0')
>>> '00000++txt'
>>> '..txt'.rjust(10, '0')
>>> '00000..txt'
```

### numbers:
```python
>>> n = 4
>>> print '%03d' % n
004
>>> print format(n, '03') # python >= 2.6
004
>>> print '{0:03d}'.format(n)  # python >= 2.6
004
>>> print '{foo:03d}'.format(foo=n)  # python >= 2.6
004
>>> print('{:03d}'.format(n))  # python >= 2.7 + python3
004
>>> print('{0:03d}'.format(n))  # python 3
004
>>> print(f'{n:03}') # python >= 3.6
004
```
>  % formatting 已被 string.format 替代

### 保留小数位：
```python
format(value, '.6f')

>>> format(2.0, '.6f')
'2.000000'
>>> '{:.6f}'.format(2.0)
'2.000000'
```