---
title: "redis"
draft: true
---

```sh
# redis 及其持久化
# redis.conf
requirepass 123456
appendonly yes
daemonize no

version: '3'
services:
  redis:
      image: redis
      restart: unless-stopped
      # command: redis-server --requirepass 123456
      command: redis-server /usr/local/etc/redis/redis.conf
      ports:
        - 6379:6379
      volumes:
        - ./redis.conf:/usr/local/etc/redis/redis.conf
        - ./data/redis:/data/
```
