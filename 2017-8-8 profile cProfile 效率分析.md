---
title: 2017-8-8 profile cProfile 效率分析
date: 2017-8-8
tags: [Python]
grammar_cjkRuby: true
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

### cProfile用法：
```python
# 生成.pstats分析文档
python -m cProfile -o profile.pstats test.py /usr
# 查看pstats文档
python -m pstats profile.pstats
# ?: 查看可用指令；sort cumtime:排序；stats:查看pstats文档
```
![enter description here][1]

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

  [1]: ./images/%E4%BB%A3%E7%A0%81%E6%AE%B5.png "sort keys"