---
title: "Python"
draft: true
---

```python
# mysql-clients
yay -S --noconfirm mysql-clients gcc
pip install mysqlclient


# json
json.dumps(item, ensure_ascii=False, indent=4)

# 对字典排序
sorted(_dict.items(), key=lambda d: d[1], reverse=False)

# unicode replace
repr()

# http server
py2 python -m SimpleHTTPServer 8000
py3 python -m http.server 8000

# 格式化输出
print("{:02d}".format(1))
print(f"{1:02d}")

# datetime
pip install python-dateutil

# yestoday
from datetime import datetime, timedelta

# days=0, seconds=0, microseconds=0, milliseconds=0, minutes=0, hours=0, weeks=0
yestoday = datetime.today() - timedelta(days=1)
print(yestoday)

from datetime import datetime
from dateutil import parser

format_time = datetime.now().strftime("%m/%d/%Y, %H:%M:%S")

t = "Thu, 9 Sep 2021 00:17:59"
result = parser.parse(t)
print(result)
print(type(result))

now = datetime.now()
print((now - result).days)

>>> import arrow
>>> arrow.get('2013-05-11T21:23:58.970460+07:00')
<Arrow [2013-05-11T21:23:58.970460+07:00]>

>>> utc = arrow.utcnow()
>>> utc
<Arrow [2013-05-11T21:23:58.970460+00:00]>

>>> utc = utc.shift(hours=-1)
>>> utc
<Arrow [2013-05-11T20:23:58.970460+00:00]>

>>> local = utc.to('US/Pacific')
>>> local
<Arrow [2013-05-11T13:23:58.970460-07:00]>

>>> local.timestamp()
1368303838.970460

>>> local.format()
'2013-05-11 13:23:58 -07:00'

>>> local.format('YYYY-MM-DD HH:mm:ss ZZ')
'2013-05-11 13:23:58 -07:00'

>>> local.humanize()
'an hour ago'

>>> local.humanize(locale='ko-kr')
'한시간 전'

# read big file
with open("log.txt") as infile:
    for line in infile:
        do_something_with(line)
        
# csv
import csv

# read
result = []
input_file = csv.DictReader(open("result.csv"))
for row in input_file:
    result.append(row)
print(result)


# write dict
my_dict = {"test": 1, "testing": 2}
with open('mycsvfile.csv', 'w', encoding="utf-8-sig") as f:  # You will need 'wb' mode in Python 2.x
    w = csv.DictWriter(f, my_dict.keys())
    w.writeheader()
    w.writerow(my_dict)

# write list
result = [{"test": 1, "testing": 2}, {"test": 1, "testing": 2}]
with open('mycsvfile.csv', 'w', encoding="utf-8-sig") as f:  # You will need 'wb' mode in Python 2.x
    w = csv.DictWriter(f, result[0].keys())
    w.writeheader()
    w.writerows(result)

# asyncio
import asyncio
import time

def now(): return time.time()

async def do_some_work(x):
    print('Waiting: ', x)

    await asyncio.sleep(x)
    return 'Done after {}s'.format(x)

start = now()

coroutine1 = do_some_work(1)
coroutine2 = do_some_work(2)
coroutine3 = do_some_work(4)

tasks = [
    asyncio.ensure_future(coroutine1),
    asyncio.ensure_future(coroutine2),
    asyncio.ensure_future(coroutine3)
]

loop = asyncio.get_event_loop()
loop.run_until_complete(asyncio.wait(tasks))

for task in tasks:
    print('Task ret: ', task.result())

print('TIME: ', now() - start)


# yield 
def create_generator(_range):
    for i in range(_range):
        yield i

result = create_generator(5)
for i in result:
    print(i)


# xmljson
import xmljson
from lxml.etree import  fromstring,tostring

json.loads(json.dumps(xmljson.badgerfish.data(fromstring(con.encode()))))

# 乘法表 
print ('\n'.join([' '.join(['%s*%s=%-2s' % (y,x,x*y) for y in range(1,x+1)]) for x in range(1,10)]))
```
