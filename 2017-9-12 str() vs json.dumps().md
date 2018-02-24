---
title: 2017-9-12 str() vs json.dumps()
date: 2017-9-12 9:39:13
categories: [Notes, Python]
tags: [Python, string, json]
---



### str() 与 json.dumps()的区别

<!--more-->

```python
>>> data = {'jsonKey': 'jsonValue',"title": "hello world"}

>>> print json.dumps(data)

{"jsonKey": "jsonValue", "title": "hello world"}

>>> print str(data)

{'jsonKey': 'jsonValue', 'title': 'hello world'}

>>> json.dumps(data)

'{"jsonKey": "jsonValue", "title": "hello world"}'

>>> str(data)

"{'jsonKey': 'jsonValue', 'title': 'hello world'}"
```

> In fact, I am more interested in their difference in single quote and double quote in output strings. It seems that I already know one difference between them (mentioned above) and whether json.loads() can load the output string. 

> json.dumps() is much more than just making a string out of a Python object, it would always produce a valid JSON string (assuming everything inside the object is serializable) following the Type Conversion Table.

> For instance, if one of the values is None, the str() would produce an invalid JSON which cannot be loaded:

```python 
>>> data = {'jsonKey': None}
>>> str(data)
"{'jsonKey': None}"
>>> json.loads(str(data))
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/System/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/json/__init__.py", line 338, in loads
    return _default_decoder.decode(s)
  File "/System/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/json/decoder.py", line 366, in decode
    obj, end = self.raw_decode(s, idx=_w(s, 0).end())
  File "/System/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/json/decoder.py", line 382, in raw_decode
    obj, end = self.scan_once(s, idx)
ValueError: Expecting property name: line 1 column 2 (char 1)
```
> But the dumps() would convert None into null making a valid JSON string that can be loaded:

```python
>>> import json
>>> data = {'jsonKey': None}
>>> json.dumps(data)
'{"jsonKey": null}'
>>> json.loads(json.dumps(data))
{u'jsonKey': None}
```

> In fact in (I believe most) implementations of Python, str(object) wraps strings in single quotes, which is not valid JSON.

```python 
An example:

In [17]: print str({"a": 1})
{'a': 1}
str(boolean) is also not valid JSON:

In [18]: print str(True)
True

__str__, can, however, be overridden in user defined classes to ensure that objects return JSON representations of themselves.
```

> 字典转字符串（dict to str）
```python
# If your dictionary isn't too big maybe str + eval can do the work:
dict1 = {'one':1, 'two':2, 'three': {'three.1': 3.1, 'three.2': 3.2 }}
str1 = str(dict1)

dict2 = eval(str1)

print dict1==dict2
# You can use ast.literal_eval instead of eval for additional security if the source is untrusted.

import json

# convert to string
input = json.dumps({'id': id })

# load to dict
my_dict = json.loads(input) 
```

> 字符串转字典（str to dict）
```python
# str to dict
In [33]: import ast
In [34]: ast.literal_eval("{'x':1, 'y':2}")
Out[34]: {'x': 1, 'y': 2}
```
> 转换已转义的字符串转字典（str to dict）

```python
>>> a = '{\\"name\\":\\"michael\\"}'
>>> print a
{\"name\":\"michael\"}

>>> type(json.loads('“' + a + '”'))
<type 'unicode'>
>>> type(json.loads(json.loads('“' + a + '”')))
<type 'dict'>
# 第一次json.loads是将里面的\"这样的字符串转为"(只有一个双引号)，第二次再将其转为一个字典，记得不要漏掉前面先加双引号。
```

> `pymongo` 根据ObjectId进行查询
```python 
from bson.objectid import ObjectId
[i for i in dbm.neo_nodes.find({"_id": ObjectId(obj_id_to_find)})]

```

> float nan

```python
>>> import math
>>> x=float('nan')
>>> math.isnan(x)
True

# The usual way to test for a NaN is to see if it's equal to itself, since nan isn't equal anything.
def isNaN(num):
    return num != num
```
