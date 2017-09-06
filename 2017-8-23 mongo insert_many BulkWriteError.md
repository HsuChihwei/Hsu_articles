---
title: 2017-8-23 mongo insert_many BulkWriteError 
date: 2017-8-23 9:31:8
categories: [Bug, Python]
tags: [Python, mongo, Bug]
---
### 问题：
> 在对mongo插入数据时，报写入问题，报错信息如下：
```
Traceback (most recent call last):
  File "/root/crs/call_history_crawler/worker/communicate.py", line 149, in insert_db_data
    if db[table].insert_many(data):
  File "/root/crs/call_history_crawler/venv/lib/python2.7/site-packages/pymongo/collection.py", line 684, in insert_many
    blk.execute(self.write_concern.document)
  File "/root/crs/call_history_crawler/venv/lib/python2.7/site-packages/pymongo/bulk.py", line 470, in execute
    return self.execute_command(sock_info, generator, write_concern)
  File "/root/crs/call_history_crawler/venv/lib/python2.7/site-packages/pymongo/bulk.py", line 314, in execute_command
    raise BulkWriteError(full_result)
BulkWriteError: batch op errors occurred
```
<!--more-->

### 分析
> 问题出现在，对同一文本进行多次插入，官方说法：` insert_many() with a list of references to a single document raises BulkWriteError`
```
>>> doc = {}
>>> collection.insert_many(doc for _ in range(10))
Traceback (most recent call last):
...
pymongo.errors.BulkWriteError: batch op errors occurred
>>> doc
{'_id': ObjectId('560f171cfba52279f0b0da0c')}

>>> docs = [{}]
>>> collection.insert_many(docs * 10)
Traceback (most recent call last):
...
pymongo.errors.BulkWriteError: batch op errors occurred
>>> docs
[{'_id': ObjectId('560f1933fba52279f0b0da0e')}]
```