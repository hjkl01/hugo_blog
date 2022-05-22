---
title: "celery"
draft: true
---

## celery 用法
```sh
# test.py
import time
from datetime import datetime

from celery import Celery


 
 
# result_address = 'elasticsearch://user:passwd@ip:port/index'

broker = 'redis://:passwd@ip:port/db'
result_address = "mongodb://user:password@ip:port/db"

app = Celery("tasks", broker=broker, backend=result_address)


def my_on_failure(exc, task_id, args, kwargs, einfo):
    print("task failed")
    for argv in [exc, task_id, args, kwargs, einfo]:
        print(argv)


app.conf.update(
    task_serializer="json",
    accept_content=["json"],  # Ignore other content
    result_serializer="json",
    timezone="Asia/Shanghai",
    enable_utc=True,
    #  下面这个就是限制tasks模块下的add函数，每秒钟只能执行10次
    #  CELERY_ANNOTATIONS = {'tasks.add':{'rate_limit':'10/s'}}
    #  或者限制所有的任务的刷新频率
    task_annotations={"tasks.add": {"rate_limit": "2/m"}},
    #  annotations={"tasks.add": {"rate_limit": "5/m", "on_failure": my_on_failure}},
    #  annotations={"*": {"rate_limit": "10/s", "on_failure": my_on_failure}},
    #  celery worker的并发数，默认是服务器的内核数目,也是命令行-c参数指定的数目
    worker_concurrency=10,
    #  celery worker 每次去BROKER中预取任务的数量
    prefetch_multiplier=1,
    #  单个任务的运行时间限制，否则会被杀死
    task_time_limit=60,
    #  压缩方案选择，可以是zlib, bzip2，默认是发送没有压缩的数据
    message_compression="zlib",
)


@app.task(default_retry_delay=30, max_retries=2, retry_kwargs={"max_retries": 3})
def add(x, y):
    print(x, y)
    #  time.sleep(3)
    return x + y


@app.task
def sleep(seconds):
    time.sleep(seconds)


@app.task
def echo(msg, timestamp=False):
    time.sleep(3)
    return "%s: %s" % (datetime.now(), msg) if timestamp else msg


@app.task
def error(msg):
    raise Exception(msg)
```

```sh
# generate.py
# https://github.com/mher/flower/blob/master/docs/api.ipynb
import requests
import json

def main():
    api_root = "http://localhost:5566/api"
    task_api = "{}/task".format(api_root)

    url = "{}/queues/length".format(api_root)
    print(url)
    resp = requests.get(url)
    print(resp.json())

    for i in range(20):
        args = {"args": [i, i**i]}
        url = "{}/async-apply/tasks.add".format(task_api)
        #  url = "{}/apply/tasks.add".format(task_api)
        print(url)
        resp = requests.post(url, data=json.dumps(args))
        reply = resp.json()
        print(reply)

        args = {"args": [i, True]}
        url = "{}/async-apply/tasks.echo".format(task_api)
        print(url)
        resp = requests.post(url, data=json.dumps(args))
        reply = resp.json()
        print(reply)
        
    #  url = "{}/result/{}".format(task_api, reply["task-id"])
    #  print(url)
    #  resp = requests.get(url)
    #  print(resp.json())
main()
```

```sh
celery -A tasks worker --loglevel=info >> logs/celery_worker.log 2>&1 &

celery -A tasks flower --loglevel=info --address=127.0.0.1 --port=5566 >> logs/celery_flower.log 2>&1 &
```
