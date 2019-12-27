---
layout: single
title: "Find last sunday of the month in Python"
date: 2019-12-27
modified:
description: "Find last sunday of the month in Python"
categories:
    - "Tech Article"
    - "Notes"
tags:
    - Python
header:
---

If you want to use cron to achieve this function, I think it is very annoying, So I choose Python

```python
import sys
import datetime
import calendar

def checkDay():
    year = datetime.date.today().year
    month= datetime.date.today().month
    day  = datetime.date.today().day
    today= str(year) + str(month) + str(day)
    last_sunday = max(week[-1] for week in calendar.monthcalendar(year, month))
    last_sunday = str(year) + str(month) + str(last_sunday)
    
    if (today == last_sunday):
        print('Today is last sunday, starting sync now.')
    else:
        print('Today is not last sunday, stopping sync now.', today, last_sunday)
        quit()

checkDay()
```