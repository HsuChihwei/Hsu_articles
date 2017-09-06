---
title: 2017-8-31 python  two sum 问题
date: 2017-8-31 17:2:59
categories: [Notes, Python]
tags: [Python, 算法]
---
### 题目描述：
> 来自`LeetCode`

```
# Time:  O(n)
# Space: O(n)

# Given an array of integers, return indices of the two numbers
# such that they add up to a specific target.
#
# You may assume that each input would have exactly one solution.
#
# Example:
# Given nums = [2, 7, 11, 15], target = 9,
#
# Because nums[0] + nums[1] = 2 + 7 = 9,
# return [0, 1].
```
<!-- more -->
### Python 解法：
```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        lookup = {}
        for i, num in enumerate(nums):
            if target - num in lookup:
                return [lookup[target - num], i]
            lookup[num] = i
        return []

    def twoSum2(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        k = 0
        for i in nums:
            j = target - i
            k += 1
            tmp_nums = nums[k:]
            if j in tmp_nums:
                return [k - 1, tmp_nums.index(j) + k]


if __name__ == '__main__':
    print Solution().twoSum((2, 7, 11, 15), 9)
	
# -*- coding: utf-8 -*-
import time
def main(a, b):
    # med = a[int(len(a)/2)]
    c=[-1, -1]
    for i, v in enumerate(a):
        for j,k in enumerate(a[i+1:]):
            c = [i, i+j+1] if v+k==b else c
    return c

def main2(a, b):
    """
    dict 存放查值
    """
    c = {}
    for i in range(len(a)):
        if b - a[i] in c:
            return [c[b-a[i]], i]
        c[a[i]] = i
    return [-1, -1]

def sum_number(a, b):
    """
    dict 存放差值
    """
    if len(a) <= 1:
        return False
    c = {}
    for i in range(len(a)):
        if a[i] in c:
            return [c[a[i]], i]
        else:
            c[b - a[i]] = i
    return [-1, -1]

def main3(a, b):
    for i in range(len(a)):
        if (b - a[len(a)-i-1]) in a[:len(a)-i-1]:
            return [a.index((b - a[len(a)-i-1])),len(a)-i-1]
    return [-1, -1]

def main4(a, b):
    try:
        # print [[a.index((b - a[len(a)-i-1])),len(a)-i-1] for i in range(len(a)) if (b - a[len(a)-i-1]) in a[:len(a)-i-1]]
        return [[a.index((b - a[len(a)-i-1])),len(a)-i-1] for i in range(len(a)) if (b - a[len(a)-i-1]) in a[:len(a)-i-1]][0]
    except:
        return [-1,-1]

def main5(a, b):
    return [[a.index((b - a[len(a)-i-1])),len(a)-i-1] for i in range(len(a)) if (b - a[len(a)-i-1]) in a[:len(a)-i-1]].pop()

def main6(a, b):
    return [[a.index(b-j), i] for i,j in enumerate(a) if a.count(b-j) > 0 and a.index(b-j)!=i].pop()

if __name__ == '__main__':
    a = range(20)
    b = 18
    # a = [49,1,2,3,50,51]
    # b=99
    print 'list:{},data:{}'.format(a,b)
    start = time.time()
    print main(a,b)
    print 'main_time_used:{}'.format(time.time()-start)
    start = time.time()
    print main2(a,b)
    print 'main2_time_used:{}'.format(time.time()-start)
    start = time.time()
    print main3(a,b)
    print 'main3_time_used:{}'.format(time.time()-start)
    start = time.time()
    print main4(a,b)
    print 'main4_time_used:{}'.format(time.time()-start)
    start = time.time()
    print main5(a,b)
    print 'main5_time_used:{}'.format(time.time()-start)
    start = time.time()
    print main6(a,b)
    print 'main6_time_used:{}'.format(time.time()-start)
```