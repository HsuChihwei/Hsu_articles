---
title: 2017-10-16 The unit test and mock in python
date: 2017-10-16 18:48:19
categories: [Notes, Python]
tags: [Python, unittest, mock, 单元测试, 挡板, 测试]
---
### mock 功效
<!--more-->

#### 完成功能测试

```python
# module.py

class Count():

    def add(self, a, b):
        return a + b

# test.py
from unittest import mock
import unittest
from module import Count

class MockDemo(unittest.TestCase):

    def test_add(self):
        count = Count()
        count.add = mock.Mock(return_value=13, side_effect=count.add)       
		# side_effect参数和return_value是相反的。它给mock分配了可替换的结果，覆盖了return_value。简单的说，一个模拟工厂调用将返回side_effect值，而不是return_value；所以，设置side_effect参数为Count类add()方法，那么return_value的作用失效。
        result = count.add(8, 8)
        print(result)
		# 将会真正的调用add()方法，得到的返回值为16（8+8）。通过print打印结果。
        count.add.assert_called_with(8, 8)
		# 检查mock方法是否获得了正确的参数。
        self.assertEqual(result, 16)

if __name__ == '__main__':
    unittest.main()
```
 
#### 解决测试依赖
> 例如，我们要测试A模块，然后A模块依赖于B模块的调用。但是，由于B模块的改变，导致了A模块返回结果的改变，从而使A模块的测试用例失败。其实，对于A模块，以及A模块的用例来说，并没有变化，不应该失败才对。这个时候就是mock发挥作用的时候了。通过mock模拟掉影响A模块的部分（B模块）。至于mock掉的部分（B模块）应该由其它用例来测试。

> add_and_multiply()函数依赖了multiply()函数的返回值
```python
# function.py
def add_and_multiply(x, y):
    addition = x + y
    multiple = multiply(x, y)
    return (addition, multiple)


def multiply(x, y):
    return x * y
	
# test.py
import unittest
import function


class MyTestCase(unittest.TestCase):

    def test_add_and_multiply(self):
        x = 3
        y = 5
        addition, multiple = function.add_and_multiply(x, y)
        self.assertEqual(8, addition)
        self.assertEqual(15, multiple)


if __name__ == "__main__":
    unittest.main()
```

> 修改multiply()函数的代码，这样测试会失败
```python
def multiply(x, y):
    return x * y + 3
```
> 把 multiply()函数mock掉，解决依赖
```python 
import unittest
from unittest.mock import patch
import function

class MyTestCase(unittest.TestCase):

    @patch("function.multiply")
	# patch()装饰/上下文管理器可以很容易地模拟类或对象在模块测试。在测试过程中，您指定的对象将被替换为一个模拟（或其他对象），并在测试结束时还原。这里模拟function.py文件中multiply()函数。
    def test_add_and_multiply2(self, mock_multiply):
		# 在定义测试用例中，将mock的multiply()函数（对象）重命名为 mock_multiply对象。
        x = 3
        y = 5
        mock_multiply.return_value = 15
		# 设定mock_multiply对象的返回值为固定的15。
        addition, multiple = function.add_and_multiply(x, y)
        mock_multiply.assert_called_once_with(3, 5)
		# 检查mock_multiply方法的参数是否正确。

        self.assertEqual(8, addition)
        self.assertEqual(15, multiple)

if __name__ == "__main__":
    unittest.main()
```

### Mock attributes in Python mock?
```python 
with patch('requests.post') as patched_post:
    type(patched_post.return_value).ok = PropertyMock(return_value=True)
```

### Mock session in requests library？

```python
# main.py
import requests

def do_session_get():
    session = requests.session()
    return session.get('foo').status_code

# tests.py
import unittest
import mock

from main import do_session_get

# The module of we mock, return the exact data that we expected
def mocked_requests(*args, **kwargs):
    # print args
    class MockResponse(object):
        def __init__(self, status_code, text, json_data=None, content=None):
            self.json_data = json_data
            self.status_code = status_code
            self.text = text
            self.content = content

        def json(self):
            return self.json_data

    if args[0] == 'foo':
        return MockResponse(200, 'success')
    elif args[0] == 'http://someurl.com/':
        return MockResponse(200, 'success')
    return MockResponse(404, '-1')
# from main import do_session_get

class TestDoSessionGet(unittest.TestCase):
    @mock.patch('requests.session')
    def test_should_mock_session_get(self, mocked_session):
        mocked_session.return_value = mock.MagicMock(get=mock.MagicMock(side_effect=mocked_requests))
        # print do_session_get()
        self.assertEqual(do_session_get(), 200)

if __name__ == '__main__':
    unittest.main()
```

### assert 常用断言速查
|   项目  |   举例  |
| --- | --- |
|assertEqual(a, b)	| a == b|
|assertNotEqual(a, b)	| a != b|
|assertGreater(a, b)	| a > b|
|assertGreaterEqual(a, b)	| a >= b|
|assertLess(a, b)	| a < b|
|assertLessEqual(a, b)	| a <= b|
|assertTrue(x)	| bool(x) is True|
|assertFalse(x)	| bool(x) is False|
|assertIs(a, b)	| a is b|
|assertIsNot(a, b)	| a is not b|
|assertIsNone(x)	| x is None|
|assertIsNotNone(x)	| x is not None|
|assertIn(a, b)	| a in b|
|assertNotIn(a, b)	| a not in b|
|assertIsInstance(a, b)	| isinstance(a, b)|
|assertNotIsInstance(a, b)	| not isinstance(a, b)|

### 干货：mock模板（requests）
```python
# common_test.py

# -*- coding: utf-8 -*-
import unittest
import mock
import requests

"""
If the module is import from others files, like:

from my.great.package import FlowType

Then, we should do this, below:

# Now we must patch 'my.great.package.requests.get'
@mock.patch('my.great.package.requests.get', side_effect=mocked_requests)

instead of "@mock.patch('common_test.requests.get', side_effect=mocked_requests)"
because the requests module was imported in that file
"""
# The module that need unittest
class FlowType(object):
    def __init__(self):
        pass

    def flow_type(self):
        url = 'http://chihweihsu.com/'
        resp = requests.get(url)
        return resp

# The data of we mock
mock_status_code = 200
mock_text = 'You are my sunshine!'
mock_json = {'data': 'You are my sunshine!'}
mock_content = 'You are my sunshine!'

# The module of we mock, return the exact data that we expected
def mocked_requests(*args, **kwargs):
    class MockResponse(object):
        def __init__(self, status_code, text, json_data=None, content=None):
            self.json_data = json_data
            self.status_code = status_code
            self.text = text
            self.content = content

        def json(self):
            return self.json_data

    if args[0] == 'http://chihweihsu.com/':
        return MockResponse(mock_status_code, mock_text, json_data=mock_json, content=mock_content)
    elif args[0] == 'http://someurl.com/':
        return MockResponse(200, 'success')
    return MockResponse(404, '-1')

# The whole test case module
class TestCommon(unittest.TestCase):
    def setUp(self):
        pass

    # We patch 'common_test.requests.get' with our own method. The mock object is passed in to our test case method.
    @mock.patch('common_test.requests.get', side_effect=mocked_requests)
    def test_flow_type(self, mocked_get):
        f = FlowType()
        resp = f.flow_type()
        self.assertIsNotNone(resp)
        self.assertEqual(resp.status_code, mock_status_code)
        self.assertIs(resp.text, mock_text)
        self.assertIn(resp.content, mock_content)
        self.assertDictEqual(resp.json(), mock_json)
        self.assertIsInstance(resp.json(), dict)

if __name__ == '__main__':
    unittest.main()
```