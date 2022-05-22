---
title: "书签管理器 shiori"
draft: true
---

```sh
# 原链接 https://github.com/go-shiori/shiori/
# loginuser: shiori 	
# passwd: gopher
version: "2.1"
services:
  shiori:
    image: nicholaswilde/shiori:latest
    container_name: shiori-default
    environment:
      TZ: Asia/Shanghai
      PUID: 1000
      PGID: 1000
      SHIORI_PG_HOST: db
      SHIORI_PG_PORT: 5432
      SHIORI_PG_USER: user
      SHIORI_PG_PASS: password
      SHIORI_PG_NAME: ""
    ports:
      - 8080:8080
    restart: unless-stopped
    volumes:
      - ./data/shiori:/data
    depends_on:
      - db
  db:
    image: postgres
    restart: always
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - ./data/shiori_postgres:/var/lib/postgresql/data
```
