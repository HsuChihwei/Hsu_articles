---
title: 2017-9-13 cProfile 
date: 2017-9-13 16:51:2
categories: [Notes, Python]
tags: [Python, cProfile, 效率]
---
```python

python -m cProfile -s tottime myscript.py

-s 选项：
'calls' (call count)
'cumulative' (cumulative time)
'cumtime' (cumulative time)
'file' (file name)
'filename' (file name)
'module' (file name)
'ncalls' (call count)
'pcalls' (primitive call count)
'line' (line number)
'name' (function name)
'nfl' (name/file/line)
'stdname' (standard name)
'time' (internal time)
'tottime' (internal time)

```