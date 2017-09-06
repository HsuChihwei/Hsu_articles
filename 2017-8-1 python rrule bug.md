---
title: 2017-8-1 python rrule bug
date: 2017-8-1
categories: [Bug, Python]
tags: [Python, Bug]
---
`http://dateutil.readthedocs.io/en/stable/_modules/dateutil/rrule.html`

### 月份日期超限 BUG
> Per RFC section 3.3.10, recurrence instances falling on invalid dates and times are ignored rather than coerced:
> Recurrence rules may generate recurrence instances with an invalid date (e.g., February 30) or nonexistent local time (e.g., 1:30 AM  on a day where the local time is moved forward by an hour at 1:00 AM).  
> Such recurrence instances MUST be ignored and MUST NOT be  counted as part of the recurrence set.

<!--more-->

This can lead to possibly surprising behavior when, for example, the start date occurs at the end of the month:
```python
>>> from dateutil.rrule import rrule, MONTHLY
>>> from datetime import datetime
>>> start_date = datetime(2014, 12, 31)
>>> list(rrule(freq=MONTHLY, count=4, dtstart=start_date))
[datetime.datetime(2014, 12, 31, 0, 0),
datetime.datetime(2015, 1, 31, 0, 0),
datetime.datetime(2015, 3, 31, 0, 0),
datetime.datetime(2015, 5, 31, 0, 0)]
```
