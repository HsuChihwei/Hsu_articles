---
title: 2017-8-10 调试 Pdb（Python debugger）
date: 2017-8-10 22:20:47
categories: [Notes, Python]
tags: [Python, Debugger]
---
## Pdb（Python debugger）：
### 命令行运行：
```python
python -m pdb my_script.py
```
<!--more-->
### 脚本内部运行：
```python
import pdb
pdb.set_trace()
```
### 常用命令：
```python 
c ：继续执行；
w：显示上下文；
a：打印当前函数参数列表；
s：单步进入，进入函数内部（step）；
n：单步跳过，不进入函数（next）；
q：退出Pdb
```