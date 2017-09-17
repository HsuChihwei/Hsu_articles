---
title: 2017-8-8 profile cProfile 效率分析
date: 2017-8-8
categories: [Notes, Python]
tags: [Python, cProfile, 效率]
---
### test.py：
```python
import os
import sys

def process(filename):
	print filename

for (dirpath, dirnames, filenames) in os.walk(sys.argv[1]):
	for filename in filenames:
		process(filename)
```
<!--more-->
### cProfile用法：
```python
# 生成.pstats分析文档
python -m cProfile -o profile.pstats test.py /usr

# 排序
python -m cProfile -s tottime myscript.py

# 查看pstats文档
python -m pstats profile.pstats
# ?: 查看可用指令；sort cumtime:排序；stats:查看pstats文档

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
![][1]

### [gprof2dot](https://github.com/jrfonseca/gprof2dot)用法:
```python
# 安装 gprof2dot
pip install gprof2dot
# 通过.pstats文档生成相应的dot文档
python -m gprof2dot -f pstats profile.pstats
# 安装graphviz（centOS系统）
sudo yum install graphviz
# 输出png文档
python -m gprof2dot -f pstats profile.pstats | dot -T png -o profile.png
```


[1]: http://ohhmsby4v.bkt.clouddn.com/image/2017-8-8%20profile%20cProfile%20%E6%95%88%E7%8E%87%E5%88%86%E6%9E%90.png